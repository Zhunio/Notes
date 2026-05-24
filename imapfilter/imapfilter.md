# iMapFilter

## 1. Install imapfilter

```bash
brew install imapfilter
```

## 2. Create imapfiler config

```lua
-- ~/.config/imapfilter/config.lua
icloud = IMAP({
	server = "imap.mail.me.com",
	username = "<email>",
	password = "<app-specific-password>",
	ssl = "tls1",
})

inbox = icloud["INBOX"]

local rules = {
	-- Password Managers
	["1password"] = "PasswordManager",

	-- Car
	["hyundai"] = "Car",
	["libertymutual"] = "Car",
	["progressive.com"] = "Car",
	["e-zpass"] = "Car",
	["onstart.com"] = "Car",

	-- Work
	["nbcuni.com"] = "Work",
	["empower"] = "Work",
	["fidelity"] = "Work",
	["unitedconcordia"] = "Work",
	["motionrecruitment"] = "Work",

	-- Travel
	["expedia"] = "Travel",
	["booking.com"] = "Travel",

	-- Taxes
	["wildwoodwealth"] = "Taxes",

	-- Education
	["linkedin"] = "Education",
	["parchment"] = "Education",
	["suny"] = "Education",
	["newpaltz"] = "Education",

	-- Immigration
	["uscis"] = "Immigration",

	-- Banking
	["capitalone"] = "Banking",
	["discover"] = "Banking",
	["ally"] = "Banking",
	["Credit Karma"] = "Banking",
	["coopgualaquiza.fin.ec"] = "Banking",

	-- Hosting
	["namecheap"] = "Hosting",
	["hostinger"] = "Hosting",

	-- Shopping
	["bestbuy.com"] = "Shopping",

	-- Spectrum
	["spectrum"] = "Spectrum",
}

for from, folder in pairs(rules) do
	inbox:contain_from(from):move_messages(icloud[folder])
end
```

## 3. Setup cron job

Create imapfilter log

```bash
mkdir -p ~/.local/state/imapfilter
```

Edit your crontab file

```bash
crontab -e
```

```bash
# Intel Mac
*/5 * * * * /usr/local/bin/imapfilter -c /Users/<user>/.config/imapfilter/config.lua >> /Users/<user>/.local/state/imapfilter/imapfilter.log 2>&1
# Apple Silicon Mac
*/5 * * * * /opt/homebrew/bin/imapfilter -c /Users/<user>/.config/imapfilter/config.lua >> /Users/<user>/.local/state/imapfilter/imapfilter.log 2>&1
```

> [NOTE]
> <user> username on your macOS
> <email> imap email
> <app-specific-password> imap password
