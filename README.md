
import praw

# List of risky keywords—expand as needed
BANNED_KEYWORDS = [
    "free", "giveaway", "NSFW", "explicit", "click here", "subscribe", "porn", "bitcoin", "crypto"
]

def is_safe_post(title, body):
    for word in BANNED_KEYWORDS:
        if word.lower() in title.lower() or word.lower() in body.lower():
            return False
    if body.count("http") > 2:
        return False
    return True

def make_post(reddit, subreddit_name, title, body):
    if not is_safe_post(title, body):
        print("This post may be flagged by Reddit filters. Please revise.")
        return None
    subreddit = reddit.subreddit(subreddit_name)
    return subreddit.submit(title, selftext=body)

# Example usage
reddit = praw.Reddit(
    client_id="YOUR_CLIENT_ID",
    client_secret="YOUR_CLIENT_SECRET",
    user_agent="SafePostBot 1.0",
    username="YOUR_USERNAME",
    password="YOUR_PASSWORD"
)

title = "Example: How to safely post on Reddit"
body = "This post avoids banned keywords and excessive links for better posting success."

post = make_post(reddit, "test", title, body)
if post:
    print(f"Success! Posted: {post.url}")
