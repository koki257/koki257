from apify_client import ApifyClient
import pandas as pd

# Initialize the ApifyClient with your API token
client = ApifyClient("apify_api_tb5ERlPewV1SBmTej8Fblh7QjgMFqS4jobPc")

# Prepare the Actor input
run_input = {
    "position": "social media",
    "country": "IN",
    "location": "TAMILNADU",
    "maxItems": 10,
    "parseCompanyDetails": True,
    "saveOnlyUniqueItems": True,
    "followApplyRedirects": True,
}

try:
    # Run the Actor and wait for it to finish
    run = client.actor("hMvNSpz3JnHgl5jkh").call(run_input=run_input)

    # Fetch the results from the run's dataset
    items = list(client.dataset(run["defaultDatasetId"]).iterate_items())

    # Check if there are items fetched
    if items:
        # Convert the data to a pandas DataFrame
        df = pd.DataFrame(items)

        # Export the DataFrame to an Excel file
        df.to_excel("indeed_scraper.xlsx", index=False)
        print("Data exported to indeed_scraper.xlsx")
    else:
        print("No items found in the dataset.")

except Exception as e:
    print(f"An error occurred: {e}")
