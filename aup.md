# Aggregated User Profile (AUP)

An AUP is everything VIA has learned about a user across all of their conversations. Each new UCP is merged into it, and it is the payload of the AUP event. Current version: **11**.

```json
{
  "AUP_VERSION": 11,
  "demographics": {
    "gender": "male",
    "customer_name": "John Smith",
    "marital_status": "married",
    "number_of_children": 2,
    "children_ages": [
      10,
      15
    ],
    "loyalty_programs": [
      {
        "program": "american express",
        "level": "platinum"
      },
      {
        "program": "british airways executive club",
        "level": null
      }
    ]
  },
  "behavioral": {
    "last_sentiment": "positive",
    "overall_sentiment": {
      "neutral": 0,
      "negative": 0,
      "positive": 5
    },
    "security": {
      "True": 0,
      "False": 5
    },
    "asked_for_coupon": {
      "True": 0,
      "False": 5
    },
    "conversation_end_stage": {
      "intent_stage": 3,
      "hotel_options_offered_but_not_chosen": 1,
      "booking_successful": 1
    }
  },
  "preferences": {
    "vacation_types": [
      "with friends",
      "couple"
    ],
    "vacation_styles": [
      "history",
      "cultural",
      "urban"
    ],
    "departure_city": {
      "current": "london",
      "previous": [
        "manchester"
      ]
    },
    "departure_airports": [
      "london city lcy"
    ],
    "destination_cities": [
      "rome",
      "porto"
    ],
    "airline_brand_preferences": [
      "ba"
    ],
    "flight_preferences": [
      "direct flights only",
      "direct flights",
      "direct flight"
    ],
    "hotel_brand_preferences": [],
    "hotel_preferences": [
      "ribeira",
      "charming boutique hotel",
      "historic center",
      "great restaurants nearby",
      "walkable streets",
      "beautiful views",
      "pool",
      "seafront",
      "close to metro rail transportation",
      "near the pantheon",
      "central location",
      "bicycle rentals",
      "safe area for young adults"
    ],
    "hotel_preference_categories": [
      "close to public transport",
      "near the beach",
      "close to restaurant",
      "sea view",
      "pool",
      "other",
      "close to nightlife",
      "close to attractions",
      "boutique hotel",
      "central location"
    ],
    "room_preferences": [
      "quiet room",
      "breakfast included",
      "king bed"
    ],
    "room_preference_counts": {
      "bed_type": {
        "king": 1
      },
      "breakfast_included": {
        "True": 1,
        "False": 0
      },
      "room_size": {},
      "view": {}
    },
    "traveling_month": [
      "july",
      "march"
    ],
    "customer_budget": [],
    "food_preferences": [
      "sushi"
    ],
    "religious_preferences": []
  },
  "bookings": {
    "trips_booked_count": 4,
    "destinations": [
      "rome",
      "porto"
    ],
    "airline_brands": [
      "az"
    ],
    "flight_category": [
      "cheapest flight"
    ],
    "hotel_names": [
      "pantheon"
    ],
    "hotel_category": [
      "comfortable experience",
      "Elegant experience"
    ],
    "room_type": [
      "quiet room",
      "family room 2 adults 2 children",
      "breakfast included",
      "king bed",
      "family room"
    ],
    "sold_products": [
      "package"
    ],
    "average_price_per_person": [
      3509.1,
      792,
      994.5
    ]
  }
}
```

## Field notes

- **Sections.** `demographics` describes the user, `behavioral` counts how conversations went, `preferences` collects what the user asked for, and `bookings` collects what they booked (only from conversations that ended in `booking_successful`).
- **Counters.** `{"True": n, "False": n}` and `{"<value>": n}` objects count conversations.
- **`demographics.loyalty_programs`** — one entry per program the user said they hold, as `{"program", "level"}`. When a later conversation states a new level for the same program, it replaces the stored one; a conversation that mentions the program without a level keeps the stored level. Empty (`[]`) for most users.
- **`preferences.room_preferences`** — room preferences across conversations as plain terms (e.g. `"king bed"`, `"breakfast included"`, `"suite"`, `"sea view"`).
- **`preferences.room_preference_counts`** — how many conversations asked for each `bed_type`, `room_size` and `view`, and for or against breakfast. Only stated preferences are counted, so the counters stay empty for users who never mention room preferences.
- **`preferences.hotel_preference_categories`** — the user's `hotel_preferences` mapped by an AI model onto a fixed list of categories (e.g. `"near the beach"`, `"five stars"`, `"family friendly"`), recalculated on every update.
- Profiles created by older versions may still carry fields that are no longer produced, such as `demographics.spouse_name`, `demographics.children_names` and `bookings.total_price`.

## Changes in version 11

- Added `demographics.loyalty_programs` and `preferences.room_preference_counts`.
- Renamed `preferences.cat_hotel_preferences` to `preferences.hotel_preference_categories`.

Version 10 removed `behavioral.stop_reason` and added `behavioral.asked_for_destination_recommendations_count` and `behavioral.asked_for_activity_recommendations_count`.
