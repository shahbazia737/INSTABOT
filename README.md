# INSTABOT
- Python v3.6 or greater
- Pip v18 or greater

## Install

**Recommended: From PyPi:** (Stable)

- `python3 -m pip install instabot-py`
## Quick Start

- **Make sure you have Python 3.6 or above installed**

  - `python3 --version`

On Windows you might have to use `python` without the version (`3`) suffix. Experienced users should use virtualenv.

- **If your version is below 3.6, we recommend you install the latest Python 3.7**

  - [Python on Windows](https://github.com/instabot-py/instabot.py/wiki/Installing-Python-on-Windows)
  - [Python on Mac](https://github.com/instabot-py/instabot.py/wiki/Installing-Python-3.7-on-macOS)
  - [Python on Raspberry Raspbian / Debian / Ubuntu](https://github.com/instabot-py/instabot.py/wiki/Installing-Python-3.7-on-Raspberry-Pi)

- **Install instabot.py from PyPi repository**

  - `python3 -m pip install instabot-py`

- **Start the bot** 🏁

  - `instabot-py`
  - or `python3 -m instabot_py`
  - For more advanced parameters and configurations download and edit [example.py](https://raw.githubusercontent.com/instabot-py/instabot.py/master/example.py) and run `python3 example.py`

- **Config** ⚙️

When you first run `instabot-py` a file called `config.ini` will be created in your current directory, along with an SQLite DB, and an error.log.

After the initial configuration, you can manually edit `config.ini` with a text editor. Restart the bot for changes to take effect.

The `%username%.db` file contains a record of the posts the bot has liked, and the users it has followed/unfollowed.

The `%username%.session` file stores your session with Instagram to avoid re-logins each time you start the bot.


## Parameters
| Parameter            | Type|                Description                           |        Default value             |
|:--------------------:|:---:|:----------------------------------------------------:|:--------------------------------:|
| login                | str | Your instagram username                              |      |
| password             | str | Your instagram password                              |      |
| start\_at\_h         | int | Start program at the hour                            | 0    |
| start\_at\_m         | int | Start program at the min                             | 0    |
| end\_at\_h           | int | End program at the hour                              | 23   |
| end\_at\_m           | int | End program at the min                               | 59   |
| database\_name       | str | change the name of database file to use multiple account | "follows\_db.db"   |
| session\_file        | str | change the name of session file so to avoid having to login every time. Set False to disable. | "username.session"   |
| like_per_day         | int | Number of photos to like per day (over 1000 may cause throttling) | 1000 |
| media_max_like       | int | Maximum number of likes on photos to like (set to 0 to disable) | 0    |
| media_min_like       | int | Minimum number of likes on photos to like (set to 0 to disable) | 0    |
| follow_per_day       | int | Users to follow per day                              | 0    |
| follow_time          | int | Seconds to wait before unfollowing                   | 5 * 60 * 60 |
| user_min_follow      | int | Check user before following them if they have X minimum of followers. Set 0 to disable                   | 0 |
| user_max_follow      | int | Check user before following them if they have X maximum of followers. Set 0 to disable                   | 0 |
| follow_time_enabled  | bool| Whether to wait seconds set in follow_time before unfollowing | True |
| unfollow_per_day     | int | Users to unfollow per day                            | 0    |
| unfollow_recent_feed | bool| If enabled, will populate database with users from recent feed and unfollow if they meet the conditions. Disable if you only want the bot to unfollow people it has previously followed. | True |
| unlike_per_day     | int | Number of media to unlike that the bot has previously liked. Set to 0 to disable.                           | 0    |
| time_till_unlike     | int | How long to wait after liking media before unliking them. | 3 * 24 * 60 * 60 (3 days) |
| comments_per_day     | int | Comments to post per day                             | 0    |
| comment_list         | [[str]] | List of word lists for comment generation. @username@ will be replaced by the media owner's username     | [['this', 'your'], ['photo', 'picture', 'pic', 'shot'], ['is', 'looks', 'is really'], ['great', 'super', 'good'], ['.', '...', '!', '!!']] |
| tag_list             | [str] | Tags to use for finding posts by hasthag or location(l:locationid from e.g. https://www.instagram.com/explore/locations/212999109/los-angeles-california/)                     | ['cat', 'car', 'dog', 'l:212999109'] |
| tag_blacklist        | [str] | Tags to ignore when liking posts                   | [] |
| user_blacklist       | {str: str} | Users whose posts to ignore. Example: `{"username": "", "username2": ""}` type only the key and leave value empty -- it will be populated with userids on startup.                | {} |
| max_like_for_one_tag | int | How many media of a given tag to like at once (out of 21) | 5 |
| unfollow_break_min   | int | Minimum seconds to break between unfollows           | 15 |
| unfollow_break_max   | int | Maximum seconds to break between unfollows           | 30 |
| log_mod              | int | Logging target (0 log to console, 1 log to file, 2 no log.) | 0 |
| proxy                | str | Access instagram through a proxy. (host:port or user:password@host:port) | |
| unfollow_not_following   | bool | Unfollow Condition: Unfollow those who do not follow you back | True |
| unfollow_inactive   | bool | Unfollow Condition: Unfollow those who have not posted in a while (inactive) | True |
| unfollow_probably_fake  | bool | Unfollow Condition: Unfollow accounts which skewed follow/follower ratio (probably fake) | True |
| unfollow_selebgram  | bool | Unfollow Condition: Unfollow (celebrity) accounts with too many followers and not enough following | False |
| unfollow_everyone  | bool | Unfollow Condition: Will unfollow everyone in unfollow queue (wildcard condition) | False |

## Methods
| Method | Description |
|:------:|:-----------:|
| get_media_id_by_tag(tag) | Add photos with a given tag to like queue |
| like_all_exist_media(num) | Like some number of media in queue |
| auto_mod() | Automatically loop through tags and like photos |
| unlike(id) | Unlike media, given its ID. |
| comment(id, comment) | Write a comment on the media with a given ID. |
| follow(id) | Follow the user with the given ID. |
| unfollow(id) | Unfollow the user with the given ID. |
| logout() | Log out of Instagram. |

<!-- ## Usage examples
Basic bot implementation:
```py
bot = InstaBot('login', 'password')
bot.auto_mod()
```

Standard use with custom tags:
```py
bot = InstaBot('login', 'password', tag_list=['with', 'your', 'tag'])
bot.auto_mod()
```

Standard use with change default settings (you should know what you do!):
```py
bot = InstaBot('login', 'password',
               like_per_day=1000,
               media_max_like=50,
               media_min_like=5,
               tag_list=['like', 'follow', 'f4f'],
               max_like_for_one_tag=50,
               log_mod=1)
bot.auto_mod()
```

Get media by one tag `'python'` and like 4 of them:
```py
bot = InstaBot('login', 'password')
bot.get_media_id_by_tag('python')
bot.like_all_exist_media(4)
```

## Video Tutorials
The following video tutorials demo setting up and running the bot:
* [Windows](https://www.youtube.com/watch?v=V8P0UCrACA0)
* [Mac/Linux](https://www.youtube.com/watch?v=ASO-cZO6uqo)
-->
