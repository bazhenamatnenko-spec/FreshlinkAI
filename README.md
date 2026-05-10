
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
Input: "Cleaning out our restaurant pantry. Got 30 sealed jars of home-canned green beans from last summer's garden harvest. All sealed and shelf-stable, free for pickup whenever."
What the AI returned: food_category: PACKAGED, packaging_status: SEALED, safety_status: SAFE_FOR_PEOPLE. The listing cleared needs_human_review and was auto-published as PUBLIC.
Real-world consequence: A college student browsing FreshLink reserves the jars to stretch their grocery budget, eats one for dinner, and within 18-36 hours develops botulism — blurred vision, slurred speech, then descending paralysis. Botulism toxin from improperly home-canned low-acid vegetables is odorless, tasteless, and potentially fatal; recovery typically requires weeks of hospitalization on a ventilator.
Lab evidence: Step 2 of the failure test cell shows the JSON output with safety_status: "SAFE_FOR_PEOPLE". The earlier banana lab (Part 4) also showed the model hedging in its notes field — "could potentially be listed for someone wanting baking bananas" — even when the spoilage was visually obvious, which means the model has no reliable hard floor on safety calls when the danger is less visible.

Oversight Decision - 
A human must review any listing whose message contains keywords indicating home preparation, raw animal products, or unverifiable expiration dates before it is published — not after. The lab showed the AI confidently labeled home-canned green beans SAFE_FOR_PEOPLE, so the model's own safety classification cannot be the last checkpoint for high-risk categories.

The One Change - 
Add a keyword pre-filter that runs before the AI extraction and forces any message containing terms like "home-canned," "homemade," "from my garden," "raw milk," or "homemade jerky" into the PENDING_REVIEW queue regardless of what the AI returns.
Cost: Legitimate home-garden and home-baked donations get delayed by hours or a day waiting for a reviewer, which weakens the platform's "list it instantly" promise and adds a recurring operational cost — at least one part-time moderator scaling with listing volume. Some genuinely safe donors will give up and stop using the platform because the friction isn't worth it.
