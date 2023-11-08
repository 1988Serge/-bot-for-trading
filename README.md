# -bot-for-trading
Creating a bot for trading cryptocurrencies using Python and the ccxt library

Шаг 1: Импорт библиотек
Мы начинаем с импорта необходимых библиотек для нашего торгового бота. В дополнение к ccxt нам также понадобятся библиотеки time и datetime.
import ccxt
import time
import datetime

Шаг 2: Настройка ключей API
Нам нужно создать ключи API для биржи Binance. Для этого нам нужно зарегистрировать учетную запись и включить доступ к API. Как только у нас будут ключи, мы сможем сохранить их в конфигурационном файле или в виде переменных окружения.
binance = ccxt.binance({
   'apiKey': 'YOUR_API_KEY',
   'secret': 'YOUR_SECRET_KEY',
})

Шаг 3: Определение торгового бота
У нашего торгового бота должна быть функция для размещения ордеров на покупку и продажу. Нам также нужно определить стратегию для нашего бота. В этом примере мы будем использовать простую стратегию покупки, когда цена падает ниже определенного порога, и продажи, когда цена поднимается выше определенного порога.
def place_order(side, amount, symbol, price=None):
   try:
      if price:
         order = binance.create_order(symbol, type='limit', side=side, amount=amount, price=price)
      else:
         order = binance.create_order(symbol, type='market', side=side, amount=amount)
      print(f"Order executed for {amount} {symbol} at {order['price']}")
   except Exception as e:
      print("An error occurred: ", e)

def trading_bot(symbol, buy_price, sell_price):
   while True:
      ticker = binance.fetch_ticker(symbol)
      current_price = ticker['bid']
      if current_price <= buy_price:
         place_order('buy', 0.01, symbol, buy_price)
      elif current_price >= sell_price:
         place_order('sell', 0.01, symbol, sell_price)
      time.sleep(60)

Шаг 4: Запуск торгового бота
Теперь мы можем запустить нашего торгового бота, вызвав функцию trading_bot с символом, ценой покупки и ценой продажи.
trading_bot('BTC/USDT', 29000, 30000)
Он будет совершать сделки по паре BTC / USDT на Binance. Бот будет постоянно проверять цену и совершать сделки в соответствии с нашей определенной стратегией.
