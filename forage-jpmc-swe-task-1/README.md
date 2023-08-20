[<img align = right height = 50 width = 50 src = https://cdn4.iconfinder.com/data/icons/social-media-and-logos-11/32/Logo_Youtube-512.png>](https://youtu.be/iCntrjJzyYc)
### # Task 1: Interface with a stock price data feed
Aim: Interface with a stock price data feed and set up your system for analysis of the data

#### Prerequisite
- [Python Installation](https://iamvishalprasad.blogspot.com/2023/06/python-installation.html#more)
- [PyCharm Installation](https://iamvishalprasad.blogspot.com/2023/06/pycharm-installation.html#more)
- [Git Installation](https://iamvishalprasad.blogspot.com/2023/06/git-installation.html#more)
- [Visual Studio Code Installation](https://iamvishalprasad.blogspot.com/2023/06/visual-studio-code-installation.html#more)
  
#### Clone your project using Command Prompt:
```ruby
git clone https://github.com/theforage/forage-jpmc-swe-task-1
cd JPMC-tech-task-1
```

#### Making changes in `client3.py` - getDataPoint Method
```ruby
price = (bid_price+ask_price) / 2
```

#### Making changes in `client3.py` – getRatio method
```ruby
return price_a to stock price_b
```

#### Making changes in `client3.py` - Main method
```ruby
print ("Ratio %s" % getRatio(price["ABC"], price["DEF"]))
```

#### If you encounter an issue with datautil.parser, run this command:
```ruby
pip install python-dateutil
```

#### Run the server app is on Administrator Mode :
```ruby
cd JPMC-tech-task-1-py3
python server3.py
```

#### Run the client app is on Command Prompt :
```ruby
cd JPMC-tech-task-1-py3
python client3.py
```

#### Bonus task: `client_test.py`
```ruby
""" ------------ Add the assertion below ------------ """
for quote in quotes:
self.assertEqual(getDataPoint(quote), (quote['stock'], quote['top_bid']['price'], quote['top_ask']['price'], (quote['top_bid']['price'] + quote['top_ask']['price'])/2))
```

#### To make patch file
```ruby
- git add -A
- git config user.email "<your_email_address>"
- git config user.name "<your_name>"
- git commit -m "Create Patch File"
- git format-patch -1 HEAD
```
