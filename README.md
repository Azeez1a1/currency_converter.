import requests

api_key = "91e04bc8b75a1eb0d4ad6d664bf5c44d"  # Replace this with your real API key

date = input("Enter the date (yyyy-mm-dd) or type 'latest': ").strip()
base = input("Convert from (currency code (Only EUR is available on this package)): ").strip().upper()
curr = input("Convert to (currency code): ").strip().upper()
quan = float(input(f"How much {base} do you want to convert: "))

# Construct URL
if date.lower() == "latest":
    url = f"http://api.exchangeratesapi.io/v1/latest?access_key={api_key}&base={base}&symbols={curr}"
else:
    url = f"http://api.exchangeratesapi.io/v1/{date}?access_key={api_key}&base={base}&symbols={curr}"

# Make the request
response = requests.get(url)

# Check for errors
if not response.ok:
    print("\nError {}:".format(response.status_code))
    print(response.json().get('error', 'Unknown error'))
else:
    data = response.json()
    rate = data['rates'].get(curr)
    if rate:
        result = quan * rate
        print(f"\n{quan} {base} is equal to {result:.2f} {curr}, based on exchange rates on {data['date']}")
    else:
        print(f"\nCurrency {curr} not found in response.")
