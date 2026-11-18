# User Conversation Profile (UCP)

A UCP is what VIA learned about a user from a single conversation. It is extracted by an AI model when a conversation ends and is the payload of the UCP event. Current version: **8**.

```json
{
  "UCP_VERSION": 8,
  "asked_about_security": false,
  "asked_for_activity_recommendations": true,
  "asked_for_coupon": false,
  "asked_for_destination_recommendations": false,
  "aup_id": "03556630-7b72-453c-8319-fcfd7602dabd",
  "average_price_per_person": 994.5,
  "budget": -1,
  "children_ages": [],
  "chosen_airline_brands": [
    "az"
  ],
  "chosen_destination_cities": [
    "rome"
  ],
  "chosen_flight_categories": [
    "cheapest flight"
  ],
  "chosen_hotel_brands": [],
  "chosen_hotel_categories": [
    "comfortable experience"
  ],
  "chosen_hotel_names": [
    "pantheon"
  ],
  "chosen_product_type": "package",
  "conversation_end_stage": "booking_successful",
  "conversation_flow": [
    {
      "intent": "romantic package to Rome in March",
      "got_proposal": true,
      "user_reaction": "asked for a direct flight instead of a connection",
      "request_change": true
    },
    {
      "intent": "same package with a direct flight",
      "got_proposal": true,
      "user_reaction": "accepted and booked",
      "request_change": false
    }
  ],
  "currency": "USD",
  "customer_name": "John",
  "departure_airport": "london city lcy",
  "departure_city": "london",
  "departure_date": "2026-03-04",
  "flight_preferences": [
    "direct flight"
  ],
  "food_preferences": [
    "sushi"
  ],
  "from_id": "447700900123",
  "gender": "male",
  "hotel_preferences": [
    "near the pantheon",
    "bicycle rentals"
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
  ],
  "marital_status": "married",
  "preferred_airlines": [
    "ba"
  ],
  "preferred_hotel_brands": [],
  "preferred_travel_month": [
    "march"
  ],
  "project_id": "",
  "religious_preferences": [],
  "requested_destination_cities": [
    "rome"
  ],
  "return_date": "2026-03-07",
  "room_preferences": {
    "bed_type": "king",
    "breakfast_included": true,
    "room_size": null,
    "view": null,
    "other": [
      "quiet room"
    ]
  },
  "sentiment": "positive",
  "synopsis": "John, a married man traveling with his wife, planned a romantic getaway to Rome from London for March 4 to 7, 2026. He holds an American Express Platinum card and asked for a king bed, breakfast included and a quiet room. He initially preferred BA but chose the cheapest direct flight, operated by AZ, from London City to Rome Fiumicino, and the Pantheon hotel in the Comfortable Experience category. He asked for things to do in Rome and completed the booking.",
  "travelers": 2,
  "ucp_id": "",
  "vacation_style": [
    "cultural",
    "history"
  ],
  "vacation_type": "couple"
}
```

## Field notes

- **Normalized text.** Descriptive text values (cities, airlines, hotels, preferences, loyalty programs) are lowercased and punctuation is replaced by spaces. `customer_name` keeps the user's original language and spelling; `synopsis` and `conversation_flow` are free English text.
- **Unknown values.** Lists are `[]`, strings are `""`, `budget` and `average_price_per_person` are `-1`, and dates that were never discussed (or the return date of a one-way trip) are `"1969-01-01"`.
- **`loyalty_programs`** — every loyalty program, frequent flyer program, hotel membership or card tier the user said they hold, as `{"program", "level"}`. `level` is `null` when the user did not state it. Most conversations mention none, so this is usually `[]`. The field is the same for every client; the program and level names vary by client.
- **`room_preferences`** — what the user asked for in a room. `bed_type` is one of `king`, `queen`, `double`, `twin`, `single`, `other`; `room_size` is one of `standard`, `large`, `suite`, `other`; `breakfast_included` is `true`/`false`; `view` is free text; anything else goes in `other`. Each part is `null` when the user did not mention it (the common case), and `other` is `[]`.
- **`conversation_flow`** — the conversation as ordered cycles of intent, proposal and reaction; `request_change: true` means the user asked for something different, which starts the next cycle.

## Changes in version 8

- Added `loyalty_programs`.
- `room_preferences` changed from a list of strings to the object above. A version 7 list maps to `other`.

Version 7 changed `vacation_style` and `conversation_flow` to lists, added `asked_for_destination_recommendations` and `asked_for_activity_recommendations`, and removed `stop_reason`.
