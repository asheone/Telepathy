## Telepathy: An OSINT toolkit for investigating Telegram chats.

Developed by Jordan Wildon. Version 2.3.4.

Telepathy is the "swiss army knife of Telegram tools" — archive chats (including replies, media, comments and reactions), gather memberlists, lookup users by location, analyze top posters, map forwarded messages, and more. Used in investigative journalism, academic research, and intelligence analysis.

**Enterprise version:** Visit [prose.ltd](https://prose.ltd) for Telepathy Pro — no accounts or CLI needed.


## Installation

```bash
# Pip (recommended)
pip3 install telepathy

# From source
git clone https://github.com/jordanwildon/Telepathy.git
cd Telepathy
pip install -r requirements.txt
```

## Setup

On first use, Telepathy asks for your Telegram API details (from [my.telegram.org](https://my.telegram.org)). It will then send an authorization code to your Telegram account. 2FA users will also be prompted for their password.

**Optional:** `pip3 install cryptg` may improve speed by offloading decryption to C.


## Usage

```
telepathy [OPTIONS]
```

### Core options

| Flag | Short | Description |
|------|-------|-------------|
| `--target TEXT` | `-t` | Target chat to investigate (use the `t.me/chatname` handle without `t.me/`) |
| `--comprehensive` | `-c` | Full scan: archives messages, reactions, forwards, reply counts, engagement rates |
| `--forwards` | `-f` | Map forwarded messages into a Gephi-compatible edgelist |
| `--media` | `-m` | Archive media files (use with `-c`). **Caution:** downloads all media |
| `--user` | `-u` | Look up a user by ID or @username (account must have "seen" the user) |
| `--location` | `-l` | Find users near coordinates: `-t 51.503,-0.121 -l` |
| `--replies` | `-r` | Archive channel replies and list replying users (use with `-c`) |
| `--translate` | `-tr` | Auto-translate messages to English during retrieval |
| `--export` | `-e` | Export all chats your account is in to CSV |
| `--alt INTEGER` | `-a` | Use alternative login credentials (0-4) |
| `--json` | `-j` | Export results to JSON |
| `--triangulate_membership` | `-tm` | Cross-reference membership across groups |

### Date filtering

| Flag | Short | Description |
|------|-------|-------------|
| `--last-days INTEGER` | `-ld` | Only fetch messages from the last N days |
| `--start-date TEXT` | `-sd` | Only fetch messages after this date (YYYY-MM-DD) |
| `--end-date TEXT` | `-ed` | Only fetch messages before this date (YYYY-MM-DD) |

### Examples

```bash
# Basic scan
telepathy -t durov

# Comprehensive scan with forwards
telepathy -t durov -c -f

# Archive media
telepathy -t durov -c -m

# Last 30 days only
telepathy -t durov -c --last-days 30

# Specific date range
telepathy -t durov -c --start-date 2025-01-01 --end-date 2025-06-01

# User lookup
telepathy -t 0123456789 -u
telepathy -t @test_user -u

# Location search
telepathy -t 51.5032973,-0.1217424 -l

# Export all chats
telepathy -e
```


## Bonus investigation tips

- Run **exiftool** on archived media to extract metadata (timestamps, timezones, authors):
  ```bash
  cd ./telepathy/telepathy_files/CHATNAME/media
  exiftool * > metadata.txt
  ```
- Use [Maigret](https://github.com/soxoj/maigret) to check where usernames from memberlists have been reused across platforms. Always verify to avoid false positives.


## How Telegram works

Telegram chats come in three types: **Channels** (broadcast, unlimited subscribers), **Megagroups/Supergroups** (up to 200K members, all can participate), and **Gigagroups** (hybrid). Each type works slightly differently with Telepathy's options.


## Usage terms

You may use Telepathy however you like, but your use case is your responsibility. Be safe and respectful. For commercial use, see [prose.ltd](https://prose.ltd).


## Credits

Created by Jordan Wildon ([@jordanwildon](https://twitter.com/jordanwildon)). Thanks to [Giacomo Giallombardo](https://github.com/aaarghhh), [jkctech](https://github.com/jkctech/Telegram-Trilateration), Alex Newhouse ([@AlexBNewhouse](https://twitter.com/AlexBNewhouse)), and [Francesco Poldi](https://github.com/pielco11).

Credit for use in published research (Jordan Wildon, Prose Intelligence, or Telepathy) is appreciated but not required.
