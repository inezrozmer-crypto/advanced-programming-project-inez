# Advanced programming project

## Ms. Match: my closet, my photo, today's outfit

A small web app I am building for myself. My clothes live in it as photos, it reads today's forecast, and one click gives me an outfit, rendered onto a photo of me, so I can see it on myself before I get dressed. It runs in the browser, with no server of my own and no account. Project brief, sections 1 to 5 below.

---

## 1. The demo

I open the app in the browser and the page asks for my location. I allow it, and the forecast panel shows today: 14°, light rain, rain 70%. The model on the page is a full-body photo of me that I uploaded once. I click **Match me**. The closet flicks through pieces for a few seconds, slows down, a handwritten "match!" pops up, and after another few seconds the photo of me changes: I am now wearing the black jeans, the brown knit sweater and the black boots from my closet, rendered by an image-editing AI onto my own photo. The "love it?" row appears. I click **Wear it today**, and the status bar says `wearing it today · 2026-11-30 saved to the calendar`. I open **Outfit calendar**: November shows the days I pressed "wear it", each with a small picture of me in that outfit, and clicking one puts it back on screen. I close the browser, reopen the app, and the closet, the saved looks, the rendered pictures and the calendar are still there.

## 2. The shape

```
in         one full-body photo of me, my clothes as photos (each with a
           category, a warmth level from 1 to 3 and a rain flag), the forecast
out        a picture of me wearing the chosen outfit, saved looks, and a
           calendar of what I wore on which day
on screen  I press "Match me" or pick pieces by hand, an image-editing AI
           puts the pieces on my photo, and I save it, wear it, or match again
```

No server of my own: everything is stored in the browser. Two outside calls: Open-Meteo for the forecast (no key) and an image-editing API for the try-on (key kept locally, never in the repository).

## 3. The size

The first useful version does:

- one photo of me as the model, and my closet as photos with category, warmth and rain flag
- forecast for my location and an outfit suggestion by warmth and rain
- "Match me" random outfit, plus manual picking
- render the pieces onto my photo through the image API, cached per outfit
- fall back to the pieces pinned as photo cards when the render fails
- save looks, "wear it today", and a monthly calendar with the pictures

Explicitly not this term:

- no accounts, no sync between devices, no server or database of my own
- no fabric-accurate fit, no video: one still picture per outfit
- no automatic cut-out of clothes: I photograph each piece on a plain background myself

## 4. How we would know it works

- Given rain probability of 50% or more, the suggested shoes are marked *rain ok* whenever such a pair exists.
- Given a dress is chosen, the bottom slot is emptied and the request to the image API contains no bottom piece.
- Given the image API fails or there is no key, the outfit is shown as pinned cards instead, and no error reaches the console.
- Given the same pieces are chosen again, the cached picture is shown without a new API request.
- Given a look is worn on day X and the page is reloaded, the calendar still shows it on day X.

## 5. What could stop this

- **The try-on.** I have never used an image-editing API for this. Quality may be poor and each render costs money. I test it in the first two weeks on three outfits, and if it is not good enough, the pinned-cards fallback becomes the final form.
- **My photos.** The real data is a photo of me and my wardrobe, sent to a third-party API. That is my own choice for my own app, and nothing personal goes into the repository.
- **Storage.** Rendered pictures will not fit in localStorage, so I will need IndexedDB, which I have not used yet.

What I could not fill in: how good the rendered pictures will look, because I have not run the image API yet. Section 4 therefore checks rules, fallback and persistence, not how convincing the picture is.
