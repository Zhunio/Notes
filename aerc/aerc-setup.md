# Aerc Setup

```sh
brew install aerc oauth2l
```

## Configuration

- `<email>`: Your email address (e.g. example@icloud.com)
- `<pass>`: Your app-specific password for iCloud email
- `<fullName>`: Your full name (e.g. John Doe)
- `~/.oauth/work.json`: OAuth 2.0 credentials file from Google Cloud Console
- `~/.cache/oauth2l`: Cache file for oauth2l tokens (will be created automatically by oauth2l)

```conf
[Work]
source            = imaps+xoauth2://<email>@imap.gmail.com:993
outgoing          = smtp+xoauth2://<email>@smtp.gmail.com:587
from              = <fullName> <email>
source-cred-cmd   = ~/.scripts/get_token.sh
outgoing-cred-cmd = ~/.scripts/get_token.sh
folder-map        = ~/.config/aerc/folder-map.conf
folders-sort      = INBOX,Important,Sent Mail,Drafts,Starred,Trash
folders-exclude   = All Mail
default           = INBOX
copy-to           = Sent Mail
cache-headers     = true

[iCloud]
source            = imaps://<email>:<password>@imap.mail.me.com:993
outgoing          = smtp://<email>:<password>@smtp.mail.me.com:587
from              = <fullName> <email>
default           = INBOX
copy-to           = Sent Messages
cache-headers     = true
folders-sort      = INBOX,Sent Messages,Archive,Trash,Junk

[Gmail]
source            = imaps://<email>:<password>@imap.gmail.com:993
outgoing          = smtp://<email>:<password>@smtp.gmail.com:587
from              = <fullName> <email>
default           = INBOX
folder-map        = ~/.config/aerc/folder-map.conf
folders-sort      = INBOX,Sent Messages,Archive,Trash,Junk
folders-exclude   = All Mail,[Mailbox],[Mailbox]/Later,[Mailbox]/To Buy,[Mailbox]/To Read,[Mailbox]/To Watch
copy-to           = Sent Mail
cache-headers     = true
```

```sh
# ~/.scripts/get-token.sh
oauth2l fetch \
--credentials ~/.oauth/work_token.json \
--scope https://mail.google.com \
--cache ~/.cache/oauth2l \
--refresh
```

```conf
# ~/.config/aerc/folder-map.conf
Spam = [Gmail]/Spam
All Mail = [Gmail]/All Mail
Drafts = [Gmail]/Drafts
Important = [Gmail]/Important
Sent Mail = [Gmail]/Sent Mail
Starred = [Gmail]/Starred
Trash = [Gmail]/Trash
```
