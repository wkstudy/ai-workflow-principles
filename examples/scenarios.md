# Real-World Scenarios

## Principle 1: Package-First Approach

**Scenario: Concurrency Control**

❌ **Custom concurrency implementation**
```javascript
// Initialize concurrency queue
let activeCount = 0, queue = []
const LIMIT = 5

// Enqueue logic
function enqueue(task) { ... }

// Dequeue logic
function dequeue() { ... }

// Core concurrency scheduler
async function schedule(fn) {
  if (activeCount >= LIMIT) {
    // Wait for a slot to open
    await waitForSlot()
  }
  activeCount++
  try {
    return await fn()
  } catch(e) {
    // Where did it fail? Is activeCount still correct?
    handleError(e)
  } finally {
    activeCount--
    // Is queue state still consistent?
    triggerNext()
  }
}

// Developer perspective:
// 80 lines of code, edge cases still not covered
// When bugs occur, unclear if activeCount or queue is broken
// New team member: what is this even doing?
```
- Large codebase, scattered logic, hard to understand at a glance
- Difficult to debug when issues arise
- High onboarding cost for new team members

✅ **Using a mature package (p-limit)**
```javascript
import pLimit from 'p-limit';
const limit = pLimit(5);
const results = await Promise.all(
  urls.map(url => limit(() => fetchPage(url)))
);

// Developer perspective:
// 3 lines of code for concurrency control
// Production-grade, no need to worry about edge case bugs
// New team member understands intent at a glance
```
- Clean code, clear intent
- Production-grade quality, thoroughly tested
- Focus on business logic, not infrastructure concerns

---

## Principle 2: UX Design for Long-Running Operations

**Scenario: Crawler Task Management**

❌ **No progress feedback** (poor user experience)
```javascript
app.post('/crawl', async (req, res) => {
  const jobs = await crawlAllJobs(req.body.keywords);
  res.json(jobs);
});

// User perspective:
// [1s]  Request submitted...
// [30s] Nothing. No idea what's happening.
// [1m]  Still waiting? Did it freeze?
// [2m]  I'll just close and refresh...
// Result: duplicate request, crawler ran twice
```
- Users anxious, unaware of progress
- Users refresh repeatedly, triggering duplicate crawls
- Network failure means complete loss of previous work
- Cannot modify parameters and resume mid-run

✅ **Full UX support** (smooth user experience)
```javascript
app.post('/crawl', async (req, res) => {
  const taskId = generateId();
  res.json({ taskId });

  // Run in background with progress feedback
  crawlJobsWithProgress(taskId, req.body, (progress) => {
    emit(`task:${taskId}:progress`, {
      processed: progress.processed,
      total: progress.total,
      status: 'crawling', // 'queued' | 'crawling' | 'paused' | 'completed'
      estimatedTime: progress.eta,
    });
  });
});

// User perspective:
// [1s]   Request submitted → received taskId
// [1-5s] Connected to WebSocket, watching live progress
//   Progress: ████████░░░░░░░░░░ 40% (40/100)
//   Estimated time remaining: 30s
// [mid-run] Want to change parameters?
//   ✓ Pause the crawler
//   ✓ Update keywords
//   ✓ Resume from checkpoint
// [done] Progress auto-saved, network failures auto-retried
```
- Users stay informed with real-time progress
- Supports pause, modify, and resume
- Automatic retry on network failure
- Users won't refresh repeatedly
