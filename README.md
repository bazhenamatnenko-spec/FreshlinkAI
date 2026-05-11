
Problem -
Businesses and restaurants discard edible surplus food every day, while many people in local communities cannot access affordable meals. Online food-sharing platforms exist, but they lack trust and transparency. Users have no way to verify a listing's food condition, expiration date, or whether the source is legitimate. The result is that safe, edible food gets wasted even when there are people nearby who need it.

AI Capability —
FreshLink AI uses generative AI for data and image extraction and automated text generation to address the trust and transparency gap. When a business posts a surplus food listing, the AI reads the listing details and any uploaded photos to categorize the food, extract key facts (type, quantity, condition, expiration), and assign a structured risk level before it goes public. This directly tackles the trust problem: instead of relying on whatever a business chooses to write, the AI standardizes every listing automatically. The prototype was adapted from Project 3 and Labs 1–3, refined with Claude, and built in Base44.

Workflow — 
A business uploads a surplus food listing with photos and details. The AI then categorizes the food, extracts key facts, and assigns it a risk level. Low-risk listings are approved automatically and go live for nearby users to browse and reserve. High-risk listings — items like cooked proteins or dairy where spoilage is harder to verify — are held and routed to a human moderator before publication. Users claim available food through a pickup reservation, and the business is notified.

<img width="499" height="259" alt="Screenshot 2026-05-08 at 4 24 05 PM" src="https://github.com/user-attachments/assets/fc5c5d7b-1339-4b72-a65c-37ed0920b75c" />
System Context Diagram: AI-Powered Food Listing Workflow

This diagram shows how the marketplace uses AI to process food listings. Businesses submit listings, which are analyzed by the AI to categorize the food and assign a risk level. Low-risk listings are automatically approved and published, while high-risk ones are sent to a human reviewer for approval. Once approved, listings become visible to users, who can then reserve pickups.

<img width="1067" height="489" alt="Screenshot 2026-05-09 at 7 16 50 PM" src="https://github.com/user-attachments/assets/70413e1a-f8bd-438e-992d-7b937db615bd" />
User Interface: Browsing and Reserving Food

This screen shows how the user can browse available food listings in the marketplace. Each listing displays key details like food type, location, pickup time, storage requirements, and risk level. Users can filter and search listings, view more details, or reserve a pickup directly from the listing.
<img width="900" height="538" alt="Screenshot 2026-05-09 at 7 17 09 PM" src="https://github.com/user-attachments/assets/aa354e79-385f-4783-a80d-e00faecb69a5" />
Human Review Interface: Moderating Food Listings

This screen shows how flagged or higher-risk food listings are reviewed by a human moderator. The reviewer can compare the original description with the AI-generated details, check important factors like food type, storage, allergens, and pickup time, and see why the listing was flagged. Based on this, they can approve, reject, or request more information before the listing goes live.


One Failure Case - 
<img width="991" height="500" alt="Screenshot 2026-05-10 at 8 02 10 PM" src="https://github.com/user-attachments/assets/af973e4a-e960-4fc5-8cb6-3320e56dfc26" />
A seller submitted a listing with a photo of bananas that had turned completely black — shriveled skin, dried stems, visibly deflated. The AI's output contradicted itself. In the FOOD_TYPE section it described the bananas as ideal for banana bread, smoothies, and muffins, calling the soft sweet flesh perfect for baking. Then in the CONDITION section, it reversed course: the bananas were extremely overripe and spoiled, the flesh likely fermented and potentially moldy, best suited for composting only. The final MARKETPLACE_ACTION landed on compost. The classification was technically right, but the same response also contained confident food-safe language. If a downstream script, integration, or human reads only the FOOD_TYPE block and acts on it, they get "ideal for banana bread" — a green light to publish as edible. The correct answer is buried under contradictory text the model generated itself.


Oversight Decision - 
A human must review any listing whose message contains keywords indicating home preparation, raw animal products, or unverifiable expiration dates before it is published — not after.  Their job is to catch internal contradictions and assess the actual product, not validate any single block of the model's output. In the banana case, the reviewer sees the conflict between FOOD_TYPE and CONDITION, looks at the photo, and resolves it: the bananas are visibly compost-only, the listing is rejected or reclassified, and the contradictory baking language never reaches a buyer or a downstream system.
The One Change - 
The cost of oversight is speed and scale. Listings don't publish instantly, and as volume grows so does the review burden — more listings means more reviewers, more payroll, more coordination. 
The benefit is that contradictions in the AI's output get caught before anyone acts on them. A reviewer looks at the actual product, so they can catch contradictions the model missed. This is very important because it protects people from getting potentially spoiled or harmful food
