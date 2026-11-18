# 🎯 LeetCode 355: Design Twitter

> **Submission for**: #DrGVishwanathanChallenge

---

### 📝 Problem Description

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and retrieve the 10 most recent tweets in the user's news feed.

Implement the `Twitter` class:
- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

#### Examples

- **Example 1**:
  - **Input**:
    ```text
    ["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
    [[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
    ```
  - **Output**: `[null, null, [5], null, null, [6, 5], null, [5]]`
  - **Explanation**: 
    - `Twitter twitter = new Twitter();`
    - `twitter.postTweet(1, 5);` // User 1 posts a new tweet (id = 5).
    - `twitter.getNewsFeed(1);`  // User 1's news feed should return a list with 1 tweet id -> [5].
    - `twitter.follow(1, 2);`    // User 1 follows user 2.
    - `twitter.postTweet(2, 6);` // User 2 posts a new tweet (id = 6).
    - `twitter.getNewsFeed(1);`  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
    - `twitter.unfollow(1, 2);`  // User 1 unfollows user 2.
    - `twitter.getNewsFeed(1);`  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.

#### Constraints

- $1 \le \text{userId}, \text{followerId}, \text{followeeId} \le 500$
- $0 \le \text{tweetId} \le 10^4$
- All tweets have unique IDs.
- At most $3 \times 10^4$ calls will be made to `postTweet`, `getNewsFeed`, `follow`, and `unfollow`.
- A user cannot follow themselves.

---

### 💡 Intuition & Strategy

1. **Object-Oriented Data Modeling**:
   - **`Tweet` Node**: Stores the tweet ID, a global logical timestamp (to order tweets globally), and a reference (`next`) to the previous tweet by the same user, forming a linked list of tweets per user.
   - **`User` Entity**: Stores a user ID, a `HashSet<Integer>` of followed user IDs, and a reference to the head (most recent) tweet in their linked list.
   - **Self-Following Default**: A user automatically "follows" themselves upon creation so their own tweets are included in their feed seamlessly.

2. **Efficient Feed Merging via Max-Heap (Priority Queue)**:
   - Getting the news feed is equivalent to **Merging $K$ Sorted Lists** (where $K$ is the number of followees).
   - Collect the `tweetHead` of each user that the target user follows.
   - Insert these heads into a **Max-Heap** ordered by timestamp in descending order.
   - Iteratively extract the top tweet (most recent overall), add its ID to the output feed, and insert its `next` tweet pointer back into the heap until 10 tweets are gathered or the heap becomes empty.

3. **Follow / Unfollow Operations**:
   - Maintained using hash sets for $O(1)$ time complexity per operation.
   - Unfollow requests for self-unfollowing are ignored.

4. **Complexity**:
   - **Time Complexity**:
     - `postTweet`: $O(1)$ to prepend to the user's tweet chain.
     - `follow` / `unfollow`: $O(1)$ lookup and modification using `HashSet`.
     - `getNewsFeed`: $O(F + K \log F)$ where $F$ is the number of users followed and $K = 10$. Initializing the heap with $F$ tweet heads takes $O(F)$ time, and pulling up to 10 most recent tweets takes $O(10 \log F)$ time.
   - **Space Complexity**: $O(U + T)$ auxiliary memory where $U$ is the total number of users created and $T$ is the total number of tweets posted.

---

### 💻 Java Solution

```java
/**
- Problem: Design Twitter (LeetCode 355)
- Language: Java 17
- Time Complexity: O(1) post/follow/unfollow, O(F + K log F) getNewsFeed (where K = 10)
- Space Complexity: O(U + T) for stored users and tweets
- Challenge: Dr. G. Vishwanathan Challenge
 */
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;
import java.util.Set;

class Twitter {
    private static int timeStamp = 0;

    // Node structure for linked list of tweets by a user
    private class Tweet {
        public int id;
        public int time;
        public Tweet next;

        public Tweet(int id) {
            this.id = id;
            this.time = timeStamp++;
            this.next = null;
        }
    }

    // User entity holding followed user IDs and tweet head pointer
    private class User {
        public int id;
        public Set<Integer> followed;
        public Tweet tweetHead;

        public User(int id) {
            this.id = id;
            this.followed = new HashSet<>();
            follow(id); // User follows themselves by default
            this.tweetHead = null;
        }

        public void follow(int id) {
            followed.add(id);
        }

        public void unfollow(int id) {
            followed.remove(id);
        }

        public void post(int id) {
            Tweet t = new Tweet(id);
            t.next = tweetHead;
            tweetHead = t;
        }
    }

    private Map<Integer, User> userMap;

    public Twitter() {
        userMap = new HashMap<>();
    }

    public void postTweet(int userId, int tweetId) {
        userMap.putIfAbsent(userId, new User(userId));
        userMap.get(userId).post(tweetId);
    }

    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res = new ArrayList<>();

        if (!userMap.containsKey(userId)) {
            return res;
        }

        Set<Integer> users = userMap.get(userId).followed;
        
        // Max-heap based on tweet timestamp descending
        PriorityQueue<Tweet> pq = new PriorityQueue<>((a, b) -> b.time - a.time);

        // Seed the priority queue with the head tweet of each followed user
        for (int user : users) {
            if (userMap.containsKey(user)) {
                Tweet t = userMap.get(user).tweetHead;
                if (t != null) {
                    pq.add(t);
                }
            }
        }

        // Retrieve up to 10 most recent tweets overall
        int count = 0;
        while (!pq.isEmpty() && count < 10) {
            Tweet t = pq.poll();
            res.add(t.id);
            count++;

            // Advance pointer for this specific user's tweet chain
            if (t.next != null) {
                pq.add(t.next);
            }
        }

        return res;
    }

    public void follow(int followerId, int followeeId) {
        userMap.putIfAbsent(followerId, new User(followerId));
        userMap.putIfAbsent(followeeId, new User(followeeId));
        userMap.get(followerId).follow(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
        if (!userMap.containsKey(followerId) || followerId == followeeId) {
            return;
        }
        userMap.get(followerId).unfollow(followeeId);
    }
}
```
