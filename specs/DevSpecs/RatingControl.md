# Input.Rating Design Specification
### Author/Editor: Brook Tamir

## ABSTRACT 
This design note covers the Rating Control that will be implemented in a future version of Adaptive Cards.

## 1. SPEC LINK
https://github.com/microsoft/AdaptiveCards/blob/specs/3054-2/specs/elements/Input.Rating.md

## 2. DEPENDENCIES

### 2.1 CHOICESET
This feature’s implementation may depend on ChoiceSet, depending on the platform.

## 3. MAJOR DECISIONS
> Note: For card authors, the rating control will be a separate control named “Input.Rating”. This is about the implementation, not what the authors see.

It makes sense to extend ChoiceSet to implement Input.Rating in the JavaScript renderer, but that decision will vary depending on platform.

## 4. BACKWARDS COMPATIBILITY CONCERNS/FALLBACK
### 4.1 Backwards Compatibility
No breaking changes to existing features or APIs are introduced with this feature.
The default fallback will be a ChoiceSet with the appropriate number of choices, so if an Input.Rating is sent to a previous version of the renderer, it will render as a ChoiceSet.

## 5. OBJECT MODEL
| Property | Type | Required | Description | Version |
| -------- | ---- | -------- | ----------- | ------- |
| **type** | `"Input.Rating"` | Yes | Must be `"Input.Rating"`. | 1.3 |
| **iconSelected** | `uri` | No | Url to selected icon, defaults to star | 1.3 |
| **iconUnselected** | `uri` | No | Url to unselected icon, defaults to star | 1.3 |
| **maxValue** | `number` | No | How many icons show up, defaults to 5 | 1.3 |

## 6. PLATFORM SPECIFIC DETAILS
### 6.1 UWP
XAML (and therefore UWP) has a native rating control that has all our required functionality. To demo, install "XAML Controls Gallery" from the Windows Store and search "RatingControl". The documentation explains how to disable the growth animation that doesn't match our specifications: https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/rating#additional-functionality
### 6.2 .NET WPF
We will leverage .NET WPF’s native rating control.
### 6.3 .NET HTML
There’s no native HTML rating control, so we may have to implement some of the interactivity in JavaScript.
### 6.4 Android
We will leverage Android’s native rating control.
### 6.5 iOS
There is no native iOS rating control, so we will have to implement it ourselves.
### 6.6 JavaScript
There’s no native JavaScript rating control, so we will have to implement it ourselves.

## 7. RENDERING DETAILS
Each platform should render the Rating input as it would any other input. Rating inputs are allowed anywhere in the card that other inputs are allowed.

## 8. TELEMETRY EVENTS
- AuthorCard
- RenderCard
- SubmitButtonClicked

## 9. TESTING
### 9.1 SAMPLES
- A simple 5-star rating case will be added under v1.3\Elements with the naming Input.Rating.json. This will demonstrate the (assumed) most common use case of sending a 1-5 star rating through an Action.Submit button.
```
{ 
  "type": "AdaptiveCard",
  "body": [ 
  	{ 
    	"type": "Input.Rating", 
    	"id": "userRating" 
      } 
    } 
  ], 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "version": "1.3" 
} 
```
- A full test file incorporating all edge cases:
	- all combinations of default and non-default number of ratings, rating icons(iconUnselected), and hover rating icons (iconSelected)
	- examples with only 1-2 rating choices
	- examples with many rating choices (enough to overfill the allotted space)
```
{ 
  "type": "AdaptiveCard", 
  "body": [ 
    { 
      "type": "Input.Rating", 
      "id": "userRating",
      "maxValue": 5,
      "iconSelected": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
      "iconUnselected": "https://pbs.twimg.com/profile_images/988775660163252226/XpgonN0X_400x400.jpg"
    },
	{ 
      "type": "Input.Rating", 
      "id": "userRating",
      "maxValue": 10
    },
	{ 
      "type": "Input.Rating", 
      "id": "userRating",
      "maxValue": 1,
      "iconSelected": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg"
    },
	{ 
      "type": "Input.Rating", 
      "id": "userRating",
      "maxValue": 2,
      "iconUnselected": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg"
    },
	{ 
      "type": "Input.Rating", 
      "id": "userRating",
      "maxValue": 20,
      "iconUnselected": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg"
    }
  ],
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "version": "1.3" 
} 
```

### 9.2 SHARED MODEL
Add the json samples to the shared model unit tests.

### 9.3 UWP
Add coverage of the json samples to the UWP test app.

## 10. SEQUENCE DIAGRAM
Author Card -> Serialize Card -> Deserialize JSON -> Parse/Validate JSON -> Render Input.Rating ->
- (UWP) use XAML Rating Control to display Input.Rating
- (iOS) custom implementation
- (JS) extend ChoiceSet
- (Android) use native rating control to display Input.Rating
- (.NET HTML) custom implementation, might use JS
- (.NET WPF) should be similar to UWP, use XAML Rating control?

[to be replaced by Visio flow diagram]