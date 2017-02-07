# [355. Design Twitter](https://leetcode.com/problems/design-twitter/)

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

1. postTweet(userId, tweetId): Compose a new tweet.
2. getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
3. follow(followerId, followeeId): Follower follows a followee.
4. unfollow(followerId, followeeId): Follower unfollows a followee.

Example:

```
   Twitter twitter = new Twitter();

   // User 1 posts a new tweet (id = 5).
   twitter.postTweet(1, 5);

   // User 1's news feed should return a list with 1 tweet id -> [5].
   twitter.getNewsFeed(1);

   // User 1 follows user 2.
   twitter.follow(1, 2);

   // User 2 posts a new tweet (id = 6).
   twitter.postTweet(2, 6);

   // User 1's news feed should return a list with 2 tweet ids -> [6, 5].
   // Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
   twitter.getNewsFeed(1);

   // User 1 unfollows user 2.
   twitter.unfollow(1, 2);

   // User 1's news feed should return a list with 1 tweet id -> [5],
   // since user 1 is no longer following user 2.
   twitter.getNewsFeed(1);
```

## Solution 1. Sort all tweet by creation time

`getNewsFeed()`

Time: O(nlgn)

A normalized memory design, so read is more expensive while write is cheap.

```java
public class Twitter {
  private final Map<Integer, Set<Integer>> follows;
  private final Map<Integer, Set<Tweet>> tweets;
  private final Set<Integer> postedTweets;
  private int counter;

  /** Initialize your data structure here. */
  public Twitter() {
    follows = new HashMap<Integer, Set<Integer>>();
    tweets = new HashMap<Integer, Set<Tweet>>();
    postedTweets = new HashSet<Integer>();
    counter = 0;
  }

  /** Compose a new tweet. */
  public void postTweet(int userId, int tweetId) {
    if (postedTweets.contains(tweetId)) return;
    postedTweets.add(tweetId);
    if (!tweets.containsKey(userId)) tweets.put(userId, new HashSet<Tweet>());
    tweets.get(userId).add(new Tweet(tweetId, counter++));
  }

  /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
  public List<Integer> getNewsFeed(int userId) {
    Set<Tweet> feed = new TreeSet<Tweet>(new Comparator<Tweet>() {
        @Override
        public int compare(Tweet a, Tweet b) {
        return a.id == b.id ? 0 : (a.time < b.time ? 1 : -1);
        }
        });
    if (tweets.containsKey(userId)) feed.addAll(tweets.get(userId));
    if (follows.containsKey(userId)) {
      for (Integer id : follows.get(userId)) {
        if (tweets.containsKey(id)) feed.addAll(tweets.get(id));
      }
    }
    List<Integer> topFeed = new ArrayList<Integer>();
    for (Tweet t : feed) {
      topFeed.add(t.id);
      if (topFeed.size() == 10) break;
    }
    return topFeed;
  }

  /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
  public void follow(int followerId, int followeeId) {
    if (!follows.containsKey(followerId)) follows.put(followerId, new HashSet<Integer>());
    follows.get(followerId).add(followeeId);
  }

  /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
  public void unfollow(int followerId, int followeeId) {
    if (!follows.containsKey(followerId)) return;
    follows.get(followerId).remove(followeeId);
  }

  private class Tweet {
    final int id, time;

    Tweet(int id, int time) {
      this.id = id;
      this.time = time;
    }
  }
}

/**
 * Your Twitter object will be instantiated and called as such:
 *  * Twitter obj = new Twitter();
 *   * obj.postTweet(userId,tweetId);
 *    * List<Integer> param_2 = obj.getNewsFeed(userId);
 *     * obj.follow(followerId,followeeId);
 *      * obj.unfollow(followerId,followeeId);
 *       */
 ```

## Solution 2. Faster feed aggregation

For each user, we order its posts by timestamp in descending order. We merge all posts to get the latest 20 posts.

Time: O(klgk) if we use a heap, or we can use mergesort to merge k sorted lists.

It is a poll model, we can also use a push model which is faster for reads but slow for writes and costs more memory.

### A more object-oriented design

Objects in question: user, tweet

```java
public class Twitter {
  private final Map<Integer, User> users;
  private final Set<Integer> posted;
  private int timestamp;

  /** Initialize your data structure here. */
  public Twitter() {
    users = new HashMap<Integer, User>();
    posted = new HashSet<Integer>();
    timestamp = 0;
  }

  /** Compose a new tweet. */
  public void postTweet(int userId, int tweetId) {
    if (!users.containsKey(userId)) users.put(userId, new User(userId));
    users.get(userId).post(tweetId);
  }

  /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
  public List<Integer> getNewsFeed(int userId) {
    if (!users.containsKey(userId)) return new ArrayList<Integer>();
    List<Integer> feed = new ArrayList<Integer>();
    Queue<Tweet> latest = new PriorityQueue<Tweet>(10, new Comparator<Tweet>() {
        @Override
        public int compare(Tweet a, Tweet b) {
        return a.id == b.id ? 0 : (a.time < b.time ? 1 : -1);
        }
        });
    for (int uid : users.get(userId).followings) {
      if (users.get(uid).tweet != null) latest.add(users.get(uid).tweet);
    }
    while (feed.size() < 10 && !latest.isEmpty()) {
      Tweet t = latest.remove();
      feed.add(t.id);
      if (t.next != null) latest.add(t.next);
    }
    return feed;
  }

  /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
  public void follow(int followerId, int followeeId) {
    if (!users.containsKey(followerId)) users.put(followerId, new User(followerId));
    if (!users.containsKey(followeeId)) users.put(followeeId, new User(followeeId));
    users.get(followerId).follow(followeeId);
  }

  /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
  public void unfollow(int followerId, int followeeId) {
    // A user cannot unfollow herself!
    if (!users.containsKey(followerId) || followerId == followeeId) return;
    users.get(followerId).unfollow(followeeId);
  }

  private class User {
    final int id;
    final Set<Integer> followings;
    Tweet tweet;

    User(int id) {
      this.id = id;
      followings = new HashSet<Integer>();
      followings.add(id);
    }

    void follow(int id) {
      followings.add(id);
    }

    void unfollow(int id) {
      followings.remove(id);
    }

    boolean post(int id) {
      if (posted.contains(id)) return false;
      posted.add(id);
      Tweet t = new Tweet(id, timestamp++);
      t.next = tweet;
      tweet = t;
      return true;
    }
  }

  private class Tweet {
    final int id, time;
    Tweet next;

    Tweet(int id, int time) {
      this.id = id;
      this.time = time;
      next = null;
    }
  }
}

/**
 * Your Twitter object will be instantiated and called as such:
 *  * Twitter obj = new Twitter();
 *   * obj.postTweet(userId,tweetId);
 *    * List<Integer> param_2 = obj.getNewsFeed(userId);
 *     * obj.follow(followerId,followeeId);
 *      * obj.unfollow(followerId,followeeId);
 *       */
 ```
