## Multiuser_Meeting_Locator

🗺️ Software concept for a group meeting location finder.

## CONCEPT

Pinpointing optimal meeting areas and locations for multiple people located in the same city, state or province.

## USAGE

Local map data is used to find addresses and to calculate route lengths.

1. Each person's location is added to a map.
2. The route distance from each person's location is calculated.
3. Optimal local meeting locations and areas are indicated on the map.

## CHATGPT CONCEPT

![ChatGPT - Concept](https://github.com/sourceduty/Multiuser_Meeting_Locator/assets/123030236/ae6539b5-d3dc-46e3-a095-641952eb00c9)

```
import googlemaps
from geopy.geocoders import Nominatim
from geopy.distance import great_circle

# Replace with your Google Maps API key
gmaps = googlemaps.Client(key='YOUR_API_KEY')

# Input addresses of participants
addresses = [
    'Address 1, City, State/Province',
    'Address 2, City, State/Province',
    'Address 3, City, State/Province',
    # Add more addresses as needed
]

# Function to geocode an address using Nominatim
def geocode_address(address):
    geolocator = Nominatim(user_agent="meeting_location_finder")
    location = geolocator.geocode(address)
    if location:
        return (location.latitude, location.longitude)
    return None

# Get the latitude and longitude of each address
coordinates = [geocode_address(address) for address in addresses]
coordinates = [coord for coord in coordinates if coord is not None]

# Function to calculate the center point
def calculate_center(coordinates):
    lats = [coord[0] for coord in coordinates]
    lons = [coord[1] for coord in coordinates]
    return sum(lats) / len(coordinates), sum(lons) / len(coordinates)

# Find the central meeting point
central_point = calculate_center(coordinates)

# Function to reverse geocode the central point to get the address
def reverse_geocode_coordinates(coordinates):
    location = gmaps.reverse_geocode((coordinates[0], coordinates[1]))
    if location:
        return location[0]['formatted_address']
    return None

# Find the address of the central meeting point
meeting_location = reverse_geocode_coordinates(central_point)

print("Central Meeting Point Coordinates:", central_point)
print("Central Meeting Point Address:", meeting_location)
```
Make sure to replace 'YOUR_API_KEY' with your actual Google Maps API key and provide the addresses of the participants in the addresses list. This code will calculate the central meeting point and provide the coordinates and the address of the meeting location.

Keep in mind that this approach finds a simple geometric center and may not account for other factors such as transportation, traffic, or preferences of the participants. Advanced algorithms and criteria may be required for more complex scenarios.
