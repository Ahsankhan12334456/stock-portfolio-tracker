import requests

class Stock:
    def __init__(self, symbol, quantity):
        self.symbol = symbol
        self.quantity = quantity
        self.price = 0.0
        self.value = 0.0

    def update_price(self):
        # Replace 'YOUR_API_KEY' with your actual API key
        api_key = 'YOUR_API_KEY'
        url = f'https://api.marketstack.com/v1/eod/latest?access_key={api_key}&symbols={self.symbol}'
        response = requests.get(url)
        if response.status_code == 200:
            data = response.json()
            if 'data' in data and len(data['data']) > 0:
                self.price = data['data'][0]['close']
                self.value = self.price * self.quantity
            else:
                print(f"Error: No data found for symbol {self.symbol}")
        else:
            print(f"Error: Failed to fetch data for symbol {self.symbol}")

    def __str__(self):
        return f"{self.symbol}: Quantity - {self.quantity}, Price - ${self.price}, Value - ${self.value}"


class Portfolio:
    def __init__(self):
        self.stocks = []

    def add_stock(self, symbol, quantity):
        stock = Stock(symbol, quantity)
        stock.update_price()
        self.stocks.append(stock)

    def update_prices(self):
        for stock in self.stocks:
            stock.update_price()

    def display_portfolio(self):
        total_value = 0.0
        for stock in self.stocks:
            print(stock)
            total_value += stock.value
        print(f"Total Portfolio Value: ${total_value}")


def main():
    portfolio = Portfolio()

    # Example: Adding stocks
    portfolio.add_stock("AAPL", 5)
    portfolio.add_stock("GOOGL", 2)

    # Update stock prices
    portfolio.update_prices()

    # Display portfolio
    portfolio.display_portfolio()


if __name__ == "__main__":
    main()# stock-portfolio-tracker
tasks no 2 stock portfolio tracker in code alpha
