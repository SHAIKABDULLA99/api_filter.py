# api_filter.py
import requests


def get_api_data(search_term=""):
    """Fetches user data from a public API and filters by name."""
    url = "https://typicode.com"

    try:
        # 1. Use requests module to fetch data
        response = requests.get(url, timeout=5)
        response.raise_for_status()

        # 2. Parse JSON response
        users = response.json()

        # 3. Add search/filter logic
        search_query = search_term.strip().lower()
        filtered_users = [
            user
            for user in users
            if search_query in user["name"].lower()
            or search_query in user["username"].lower()
        ]

        # Display results properly
        print(f"\n[Search Results for: '{search_term}']")
        print(f"Found {len(filtered_users)} matching users.\n")
        print(f"{'ID':<5} | {'Name':<25} | {'Email'}")
        print("-" * 60)

        for user in filtered_users:
            print(f"{user['id']:<5} | {user['name']:<25} | {user['email']}")

    except requests.exceptions.RequestException as e:
        print(f"Error fetching data: {e}")


if __name__ == "__main__":
    # Test case 1: Search for users containing "Clement"
    get_api_data(search_term="Clement")

    # Test case 2: Clear search (returns everyone)
    # get_api_data(search_term="")
    
