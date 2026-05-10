FreshLinkAI: A marketplace for food waste

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


Failure Case — One specific failure, with a reference to the lab output that showed it is possible.
The AI could incorrectly approve an unsafe food listing — for example, misreading a photo of spoiled produce as acceptable, or failing to flag a listing where the expiration date has passed but is not clearly stated. This failure mode is possible because image-based food safety assessment is an imprecise task; the lab outputs showed that AI extraction from ambiguous or low-quality photos can produce confident but incorrect classifications.

Oversight and Tradeoff — Where does human review sit, and what does the one change cost?
Human review sits after AI risk classification but before any listing goes live. Any listing the AI flags as high-risk, or where confidence is uncertain, is held for a human moderator to approve or reject. This keeps AI in the loop for speed and scale while reserving safety-critical decisions for humans.

The tradeoff: Adding human review at Step 4 slows down the listing approval process for flagged items. A business posting high-risk food (e.g., cooked proteins, dairy) may experience a delay before their listing appears publicly, which could reduce participation from time-sensitive donors. The alternative — fully automated approval — would be faster but risks letting unsafe food reach users, which is the more serious harm.

