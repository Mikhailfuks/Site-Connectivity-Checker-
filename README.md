import requests
import socket

def check_site_connectivity(url):
    """Checks if a website is accessible and returns the status code."""

    try:
        response = requests.get(url, timeout=5) # Set a timeout of 5 seconds
        response.raise_for_status() # Raise an exception for bad status codes (4xx or 5xx)
        return response.status_code, "OK" #Return status code and OK if successful
    except requests.exceptions.RequestException as e:
        if isinstance(e, requests.exceptions.Timeout):
            return None, "Timeout"
        elif isinstance(e, requests.exceptions.ConnectionError):
            return None, "Connection Error"
        elif isinstance(e, requests.exceptions.HTTPError):
            return response.status_code, "HTTP Error" #Specific HTTP error
        else:
            return None, f"Unknown Error: {e}"
    except socket.gaierror:
        return None, "DNS Resolution Error" # Problem resolving the hostname
    except Exception as e:
      return None, f"An unexpected error occurred: {e}"



if __name__ == "__main__":
    url = input("Enter the URL to check: ")
    try:
        status_code, status_message = check_site_connectivity(url)
        if status_code:
            print(f"Status Code: {status_code} - {status_message}")
        else:
            print(f"Website not accessible: {status_message}")
    except requests.exceptions.InvalidURL:
        print("Invalid URL format")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

