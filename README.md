# Assigmnment-ReactNative

## Objective
I want you to build the homepage shown in the shared Figma using React Native.

I want you to focus on:
- homepage rendering
- UI quality and polish
- the important animation and interaction details shown in the reference video
- mapping the homepage from the provided local fixture data

You do not need real backend integration. Use only the local fixture file I share.

## What I Will Share
- Figma for the homepage:
  File link: https://www.figma.com/design/gWSL2i0XOGlUNNoZRziWdW/Project?node-id=10-800&t=RuFUxxaJr8QrczaD-0
  Aniamiton link (assignment-rn.m4v attacehd for ref): https://www.figma.com/make/9dd7HiNr3mbPjQBDo0lWzV/Scrolling-Sticky-Sections-Prototype?code-node-id=0-9&p=f&t=18dZAPQg5aZJ2lf2-0&fullscreen=1
- this assignment document
- local fixture pack: `docs/homepage-assignment-candidate-fixtures.json`

## Data Included
The fixture file already contains the local data you need:
- `serviceability`: 1 response
- `feed_config`: 1 response
- `curated_feed_Food_item`: 10 meal-for-one food items
- `curated_feed_res_item`: 10 restaurant items for horizontal curated lists
- `curated_list_details`: 20 category / cuisine items
- `past_orders`: 8 reorder restaurant items
- `paginated_restaurant_feed`: 10 restaurant items

## Use This As Your Single Data Source
Use `docs/homepage-assignment-candidate-fixtures.json` as the only local data source.

Map the fixture keys like this:

| Fixture Key | API Name | Use In Homepage |
| --- | --- | --- |
| `serviceability` | `POST /serviceability/check` | serviceability gate before showing homepage |
| `feed_config` | `GET /feed-configs/current-feed-config` | top banner, reorder config, meal-for-one config, "What's on your mind?" config |
| `curated_feed_Food_item` | `POST /getFeedForCuratedList` | meal-for-one curated response |
| `curated_feed_res_item` | `POST /getFeedForCuratedList` | horizontal restaurant curated response |
| `curated_list_details` | `POST /getCuratedListDetails` | "What's on your mind?" / cuisine-category grid items |
| `past_orders` | `POST /get-feed-for-user-past-orders` | reorder cards |
| `paginated_restaurant_feed` | `POST /getPaginatedRestaurantFeed` | main restaurant listing |

## Exact Fixture Keys You Should Read
- serviceability gate → `serviceability.data`
- top banner → `feed_config.data.restaurant.topBanner`
- reorder config → `feed_config.data.restaurant.reOrderConfig`
- meal-for-one config → `feed_config.data.mealForOne.curatedListDetailsList`
- restaurant curated section definitions → `feed_config.data.restaurant.curatedListDetailsList`
- "What's on your mind?" config → `feed_config.data.restaurant.curatedListGroups`
- meal-for-one cards → `curated_feed_Food_item.data.FoodItems`
- restaurant curated section cards → `curated_feed_res_item.data.EntityResults`
- "What's on your mind?" items → `curated_list_details.data`
- reorder cards → `past_orders.data.EntityResults`
- main restaurant list → `paginated_restaurant_feed.data.EntityResults`

## Homepage Flow
I want you to build the homepage using this flow:

1. Check `serviceability` first.
2. If `isServiceable` is `true`, load the homepage.
3. Read `feed_config`.
4. Render the top banner from `feed_config`.
5. Render reorder using `feed_config.restaurant.reOrderConfig` + `past_orders`.
6. Render meal-for-one using `feed_config.mealForOne.curatedListDetailsList` + `curated_feed_Food_item`.
7. Render the restaurant curated sections using `feed_config.restaurant.curatedListDetailsList` + `curated_feed_res_item`.
8. Render "What's on your mind?" using `feed_config.restaurant.curatedListGroups` + `curated_list_details`.
9. Render the main restaurant listing using `paginated_restaurant_feed`.

If a section has no usable data, you can hide it unless the Figma clearly requires an empty state.

## UI Expectations
UI quality is an important part of this assignment.

I will evaluate:
- how closely you match the Figma
- spacing, alignment, typography, colors, border radius, and image treatment
- overall polish of the homepage
- whether the important animations and transitions from the reference video are implemented well
- whether the homepage feels smooth and intentional, not just functionally correct

What I expect:
- build the UI close to the design, not just approximate blocks
- keep section hierarchy visually clean
- make banners, cards, lists, and headers feel polished
- keep scrolling smooth
- pay attention to small details like padding, spacing, truncation, and image treatment
- handle loading, empty, and non-serviceable states in a visually reasonable way

You do not need to build flows outside the homepage, but the homepage itself should feel production-like.

## Scope
- render the homepage from Figma
- use the provided fixture file as mocked API data
- show the homepage only when serviceability is positive
- map each homepage section to the correct fixture key
- implement the important animation and interaction details shown in the reference video

##NOTES FOR STICKY ANIMATION
- make sure both whats on your mind list and filter get stick when user scroll past that componenet.
- and while coming back it agian get removed accordingly.
- try to make its attachment and deattachment smooth.
- for better ref look in any food delivery app for same.

## Section Mapping

### Top Banner
Use:
- `feed_config.data.restaurant.topBanner`

What to read:
- banner image

### Reorder
Use:
- config: `feed_config.data.restaurant.reOrderConfig`
- cards: `past_orders.data.EntityResults`

What to read:
- `entityId`
- `name`
- `imageUrl`
- `brandLogo`
- `etaInMinutes`
- `distanceInKM`
- `displayTags`
- `platformRating`

### Meal For One
Use:
- config: `feed_config.data.mealForOne.curatedListDetailsList`
- cards: `curated_feed_Food_item.data.FoodItems`

What to read:
- `foodItemId`
- `name`
- `resName`
- `imageUrl`
- `price`
- `vegOrNonVeg`
- `hasVariants`
- `etaInMinutes`
- `ResRatingResponse`

### Restaurant Curated Sections
Use:
- config: `feed_config.data.restaurant.curatedListDetailsList`
- cards: `curated_feed_res_item.data.EntityResults`

In this homepage config, this appears as two restaurant curated sections:
- `69ca7d7d8acad0b44091c0d0` → `Great Food, Better Prices`
- `699c21c8a934e7899c2d245a` → `Known & Loved`

Important note:
- in the real homepage flow, `getFeedForCuratedList` is called 3 times total
- 1 call for meal-for-one
- 2 calls for the restaurant curated sections above
- the same API is reused with different ids
- `curatedListDetailsList[].name` is the section title you should render
- `curatedListDetailsList[].id` is the curated id used to fetch that section's data
- for this assignment, use `curated_feed_res_item` as the local restaurant curated response fixture

### What's On Your Mind?
Use:
- config: `feed_config.data.restaurant.curatedListGroups`
- items: `curated_list_details.data`

How to treat this section:
- `curatedListGroups` tells you the group exists and gives you the ordered list of ids
- `curated_list_details` gives you the display data for each id
- use this to render the homepage cuisine / category grid

What to read:
- `id`
- `name`
- `imageUrl`
- `curatedListIds`

### Main Restaurant Listing
Use:
- `paginated_restaurant_feed.data.EntityResults`

What to read:
- `entityId`
- `name`
- `imageUrl`
- `etaInMinutes`
- `distanceInKM`
- `platformRating`
- `orderingEnabled`
- `knownFor`

## Real Fixture JSON You Should Use

### `serviceability.data`
```json
{
  "isServiceable": true,
  "reason": "SERVICEABLE",
  "message": "Service is available"
}
```

### `feed_config.data`
```json
{
  "mealForOne": {
    "id": "69f050ad5a0f80cb86270edd",
    "name": "Bel&Sarj_Dinner_Exp",
    "section": "FOOD_ITEM",
    "curatedListDetailsList": [
      {
        "id": "69f050d15a0f80cb86270edf",
        "name": "Bestsellers under 149",
        "imageUrl": "https://storage.googleapis.com/pd-onl-app-assets/best_sellers_under_149.png",
        "backGroundColour": "linear-gradient(137deg, #FCFCFC -17.38%, #F7ECDF 97.7%)",
        "section": "FOOD_ITEM",
        "rank": 1,
        "deepLinkUrl": "",
        "coverImageUrl": "",
        "subText": "",
        "curatedListType": "TOPDEALS",
        "foodType": ""
      }
    ]
  },
  "restaurant": {
    "id": "69f050325a0f80cb86270edc",
    "name": "Exp_Bel&Sar_SNACKS_20/04",
    "section": "RESTAURANT",
    "topBanner": {
      "banners": [
        {
          "id": "69ed6e4fb6c4df4d0c28c884",
          "name": "Great Food, Better Prices",
          "curatedListId": "69ca7c778acad0b44091c0cf",
          "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/curated-list/curated-list-grouptop_banner.png",
          "backGroundColour": "",
          "gifUrl": "",
          "feedConfigId": "",
          "deepLinkUrl": ""
        }
      ],
      "rank": 0,
      "section": "RESTAURANT"
    },
    "curatedListGroups": [
      {
        "id": "000000000000000000000000",
        "name": "What's on your mind?",
        "imageUrl": "",
        "backGroundColour": null,
        "curatedListIds": [
          "687a2610a05189d0e37e7ab4",
          "687a26e5a05189d0e37e7ab7",
          "687a2779a05189d0e37e7ab8",
          "687a27e5a05189d0e37e7aba",
          "687a2845a05189d0e37e7abc",
          "687a28c3a05189d0e37e7abd",
          "687a2948a05189d0e37e7abe",
          "687a29a5a05189d0e37e7abf",
          "687a29cca05189d0e37e7ac0",
          "689b8c514b5ecde5e3dd0d93",
          "68cbaab84e1601b7f79f18b2",
          "68ef4d0b9314bf7e34fcbfb3",
          "69008dc41ef8004ee099600c",
          "690b2d68ebebd5850cddceaf",
          "690b395aebebd5850cddceb2",
          "698b22025cc399dd2d3aa3be",
          "698b25915cc399dd2d3aa3c2",
          "698b26335cc399dd2d3aa3c4",
          "698b2cbe5cc399dd2d3aa3c6",
          "698b2d6c5cc399dd2d3aa3c8"
        ],
        "rank": 2,
        "section": "RESTAURANT",
        "length": 0,
        "width": 0
      }
    ],
    "curatedListDetailsList": [
      {
        "id": "699c21c8a934e7899c2d245a",
        "name": "Known & Loved",
        "imageUrl": "",
        "backGroundColour": "",
        "section": "RESTAURANT",
        "rank": 3,
        "deepLinkUrl": "",
        "coverImageUrl": "",
        "subText": "",
        "curatedListType": "",
        "foodType": ""
      },
      {
        "id": "69ca7d7d8acad0b44091c0d0",
        "name": "Great Food, Better Prices",
        "imageUrl": "",
        "backGroundColour": "",
        "section": "",
        "rank": 2,
        "deepLinkUrl": "",
        "coverImageUrl": "",
        "subText": "",
        "curatedListType": "",
        "foodType": ""
      }
    ],
    "reOrderConfig": {
      "name": "",
      "rank": 1,
      "imageUrl": "",
      "backGroundColour": ""
    }
  },
  "timeRange": {
    "from": 1080,
    "to": 1140
  }
}
```

### `curated_feed_Food_item.data`
```json
{
  "SearchId": "",
  "TotalCount": 50,
  "EntityResults": null,
  "FoodItems": [
    {
      "foodItemId": "RN414125PP139120865",
      "resId": "RN414125",
      "resName": "Sri Lakshmi Biryani",
      "name": "BABY Special Boneless Chicken Biryani(Small.)",
      "price": 98,
      "displayPrice": 98,
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/menu-images/0hod98xv22_item_RN414125PP139120865_20260616234127.jpeg",
      "vegOrNonVeg": "non-veg",
      "hasVariants": false,
      "distanceInKm": 2.75,
      "etaInMinutes": 37,
      "ResRatingResponse": {
        "type": "platform",
        "value": 3.3613325158389555,
        "count": 4843
      },
      "resImageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/RN414125.png"
    },
    {
      "foodItemId": "RN128656UP113117543",
      "resId": "RN128656",
      "resName": "Burger It Up",
      "name": "Chicken Snacker Burger",
      "price": 56,
      "displayPrice": 56,
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/menu-images/17614_item_RN128656UP113117543_20260613210705.jpg",
      "vegOrNonVeg": "non-veg",
      "hasVariants": true,
      "distanceInKm": 6.13,
      "etaInMinutes": 49,
      "resImageUrl": ""
    }
  ]
}
```

### `curated_feed_res_item.data`
```json
{
  "SearchId": "",
  "TotalCount": 10,
  "EntityResults": [
    {
      "entityId": "RN124580",
      "name": "Imperio Restaurant - Since 2010",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/ownly_6918386561761_item_RN124580DP2319_20260417203310.PNG",
      "price": 150,
      "etaInMinutes": 51,
      "distanceInKM": 5.2,
      "platformRating": {
        "value": 3.843680709534368,
        "count": 1754
      },
      "orderingEnabled": true
    },
    {
      "entityId": "RN123905",
      "name": "Biryani By Kilo",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/RN126838.jpg",
      "price": 240,
      "etaInMinutes": 30,
      "distanceInKM": 1.3,
      "platformRating": {
        "value": 4.0410094637223875,
        "count": 901
      },
      "orderingEnabled": true
    }
  ],
  "FoodItems": null
}
```

### `curated_list_details.data`
```json
[
  {
    "id": "687a2610a05189d0e37e7ab4",
    "name": "North Indian",
    "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/curated-list/curated-listnorth-indian-thali.png"
  },
  {
    "id": "687a26e5a05189d0e37e7ab7",
    "name": "Biryani",
    "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/curated-list/curated-listBiryani.png"
  },
  {
    "id": "687a2779a05189d0e37e7ab8",
    "name": "Chinese",
    "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/curated-list/curated-listchinese.png"
  },
  {
    "id": "687a27e5a05189d0e37e7aba",
    "name": "Chicken",
    "imageUrl": "https://storage.googleapis.com/pd-onl-rtn-media/curated-list/chicken.png"
  }
]
```

### `past_orders.data`
```json
{
  "SearchId": "",
  "TotalCount": 0,
  "EntityResults": [
    {
      "entityId": "RN128392",
      "name": "Burger It Up",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/RN128287.jpg",
      "brandLogo": "https://pd-onl-rtn-media.storage.googleapis.com/curated-list/brand-logoBR112545.png",
      "etaInMinutes": 29,
      "distanceInKM": 0.9,
      "displayTags": [
        "Ordered 4 months ago"
      ],
      "platformRating": {
        "value": 3.9027454630060494,
        "count": 2099
      },
      "orderingEnabled": true
    },
    {
      "entityId": "RN133504",
      "name": "Sharief Bhai Biryani",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/BR112583.png",
      "brandLogo": "https://storage.googleapis.com/pd-onl-rtn-media/brand_logos/sharief%20bhai.png",
      "etaInMinutes": 41,
      "distanceInKM": 4.1,
      "displayTags": [
        "Ordered 2 months ago"
      ],
      "platformRating": {
        "value": 3.9586776859504136,
        "count": 1402
      },
      "orderingEnabled": true
    }
  ]
}
```

### `paginated_restaurant_feed.data`
```json
{
  "SearchId": "",
  "TotalCount": 1140,
  "EntityResults": [
    {
      "entityId": "RN414042",
      "name": "Paratha Corner",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/_item_RN414042_95213_20260127173123.jpg",
      "price": 120,
      "etaInMinutes": 50,
      "distanceInKM": 6.4,
      "knownFor": [
        "paratha",
        "paratha"
      ],
      "platformRating": {
        "value": 3.937974683544304,
        "count": 740
      },
      "orderingEnabled": true
    },
    {
      "entityId": "RN417788",
      "name": "The Breakfast Nook",
      "imageUrl": "https://pd-onl-rtn-media.storage.googleapis.com/rnt-images/TBN%20thumbnail.jpg",
      "price": 190,
      "etaInMinutes": 49,
      "distanceInKM": 6.4,
      "knownFor": [
        "breakfast",
        "eggs"
      ],
      "platformRating": {
        "value": 4.28679817905919,
        "count": 659
      },
      "orderingEnabled": true
    }
  ]
}
```

## Suggested Implementation Order
I recommend you build this in this order:

1. Set up local data access from `docs/homepage-assignment-candidate-fixtures.json`.
2. Add serviceability handling.
3. Build the static homepage shell from Figma.
4. Add the top banner.
5. Add reorder.
6. Add meal-for-one.
7. Add the restaurant curated sections.
8. Add the "What's on your mind?" grid.
9. Add the main restaurant listing.
10. Add loading, empty, and non-serviceable states.
11. Add the important motion and interaction polish from the reference video.

## Submission Expectations
- runnable React Native project  and video recording
- clear run instructions
- mocked response data included in the solution
- short assumptions note if anything was simplified

## Evaluation
I will primarily look at:
- UI fidelity to Figma
- quality of animations and interactions from the reference video
- correctness of fixture-to-component mapping
- code structure and clarity
- handling of serviceability, loading, and empty states
