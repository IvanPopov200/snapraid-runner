# Snapraid Runner Script

This script runs snapraid and sends its output to the console, a log file,
email, and Telegram bot notifications. All this is configurable.

It can be run manually, but its main purpose is to be run via cronjob/windows
scheduler.

It supports Windows, Linux and macOS and requires at least python3.7.

## How to use
* If you don’t already have it, download and install
  [the latest python version](https://www.python.org/downloads/).
* Download [the latest release](https://github.com/Chronial/snapraid-runner/releases)
  of this script and extract it anywhere or clone this repository via git.
* Copy/rename the `snapraid-runner.conf.example` to `snapraid-runner.conf` and
  edit its contents. You need to at least configure `snapraid.executable` and
  `snapraid.config`.
  * [The wiki](https://github.com/Chronial/snapraid-runner/wiki/How-to-use-snapraid-runner-with-gmail)
    has details on how to use gmail for sending mail.
  * For Telegram notifications, create a bot with
    [@BotFather](https://t.me/BotFather), then configure the `[telegram]`
    section with `sendon`, `bot_token`, and `chat_id`.
* Run the script via `python3 snapraid-runner.py` on Linux or
 `py -3 snapraid-runner.py` on Windows.

## Features
* Runs `diff` before `sync` to see how many files were deleted and aborts if
  that number exceeds a set threshold.
* Can create a size-limited rotated logfile.
* Can send notification emails and Telegram bot messages after each run or only
  for failures.
* Can run `scrub` after `sync`

## Telegram notifications
Telegram support uses only Python standard library modules. To enable it:

* Create a bot with [@BotFather](https://t.me/BotFather) and copy the token.
* Start a chat with the bot, or add it to the target group/channel. For
  channels, add the bot as an administrator.
* Set `[telegram] sendon = success,error`, `bot_token = ...`, and `chat_id = ...`
  in `snapraid-runner.conf`.
* If you only want Telegram notifications, leave `[email] sendon` empty.

`chat_id` can be a numeric chat ID or a channel username such as
`@channelusername`. To find a numeric chat ID, send the bot a message and open
`https://api.telegram.org/bot<TOKEN>/getUpdates`, then use the
`message.chat.id` value from the response. Telegram messages are sent through
the Bot API
[`sendMessage`](https://core.telegram.org/bots/api#sendmessage) method, so
messages are capped at Telegram's 4096 character limit and long logs are
shortened automatically.

## Scope of this project and contributions
Snapraid-runner is supposed to be a small tool with clear focus. It should not
have any dependencies to keep installation trivial. I always welcome bugfixes
and contributions, but be aware that I will not merge new features that I feel
do not fit the core purpose of this tool.

I keep the PRs for features I do not plan on merging open, so if there's a
feature you are missing, you can have a look
[at the open PRs](https://github.com/Chronial/snapraid-runner/pulls).

## Changelog
### Unreleased
* Add Telegram bot notifications using the Python standard library.
* Add --ignore-deletethreshold (by exterrestris, #25)
* Add support for scrub --plan, replacing --percentage (thanks to fmoledina)
* Remove snapraid progress output. Was accidentially introduced with python3
  support.

### v0.5 (26 Feb 2021)
* Remove (broken) python2 support
* Fix snapraid output encoding handling (by hyyz17200, #31)
* Fix log rotation (by ptoulouse, #36)

### v0.4 (17 Aug 2019)
* Add compatibility with python3 (by reed-jones)
* Add support for running `snapraid touch` (by ShoGinn, #11)
* Add SMTP TLS support

### v0.3 (20 Jul 2017)
* Limit size of sent emails

### v0.2 (27 Apr 2015)
* Fix compatibility with Snapraid 8.0
* Allow disabling of scrub from command line

### v0.1 (16 Feb 2014)
* Initial release
