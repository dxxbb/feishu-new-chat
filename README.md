# feishu-new-chat

Use a Feishu topic-group like ChatGPT **New Chat**.

This skill creates a **new topic** in a Feishu topic-group, and can optionally post the **first in-thread reply** inside that topic, with optional **@mention** support.

## What it does

Validated workflow:

1. Send a **top-level message** to a Feishu topic-group → creates a **new topic**
2. Reply with `reply_in_thread=true` → posts **inside that topic**
3. Include Feishu text mention markup like `<at user_id="ou_xxx">Name</at>` → **@mention** a user in the thread reply

This makes a Feishu topic-group usable as a lightweight multi-thread conversation surface, similar to ChatGPT's **New Chat** pattern.

## Use cases

- `开话题：标题`
- `/topic 标题`
- `开话题：标题｜先发一条线程回复`
- `新开一个话题，然后在话题里回复一条`
- `开话题：测试｜@我 回复一条`

## Preconditions

This works correctly only when all of the following are true:

- The destination is a real **Feishu topic-group**
- User-auth Feishu messaging is available
- The sending user has permission to post in the target group
- The target group can be resolved by name or known `chat_id`

If the destination is a normal Feishu group, the top-level send will only become a normal group message, not a new topic.

## Repository layout

```text
feishu-new-chat/
├── skills/
│   └── feishu-topic-spawn/
│       └── SKILL.md
└── dist/
    └── feishu-topic-spawn.skill
```

## Install

### Option 1: import the packaged skill

Use the packaged file in `dist/feishu-topic-spawn.skill`.

### Option 2: copy the skill source

Copy `skills/feishu-topic-spawn/` into your skills directory.

## Implementation notes

The validated tool path is:

- `feishu_im_user_message.send` → create the topic root
- `feishu_im_user_message.reply` with `reply_in_thread=true` → create the first thread reply
- Feishu text mention markup → optional `@user`

This repo intentionally documents the **working path**, not an imagined one.

## Status

Tested in a real Feishu topic-group with these behaviors confirmed:

- topic-only creation
- topic + first thread reply
- topic + first thread reply + `@me`
- topic + first thread reply without `@`

## Future improvements

- publish to ClawHub
- add better parsing for title/body/@mention patterns
- add runtime checks for token/auth/group-type mismatches
