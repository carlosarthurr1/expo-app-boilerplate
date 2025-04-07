1. Project Setup
Create Expo Project

Use npx create-expo-app (or a similar command) to set up a fresh Expo app.

Pick TypeScript or JavaScript depending on your preference.

Install Supabase and Other Dependencies

npm install @supabase/supabase-js

Add packages for navigation (e.g., @react-navigation/native, react-native-screens, etc.).

Add payment-related packages (Stripe or a similar provider) to handle subscriptions.

2. Supabase Configuration
Initialize Supabase Client

In a shared config file (e.g., supabase.js or lib/supabase.js), set up the Supabase client with your self-hosted URL and public key.

Set Up Auth Table and Policies

Ensure users can register and log in using email or phone (depending on your preference).

Set Row-Level Security (RLS) rules so that user data remains private.

Check Database Structure

Create tables for:

users (if you need extra user details beyond the default Supabase auth).

profiles for storing relationship type, kinks, and other preferences.

sessions or requests for logging ring actions and coin toss results.

3. Authentication Flow
UI for Login and Registration

Have a screen that lets users log in or sign up.

Use Supabase’s signUp and signIn methods.

After registration, send the user to the onboarding flow.

Session Persistence

Listen for auth state changes so the user stays logged in or gets logged out properly.

4. Paid Access and Free Trials
Subscription Plans

Monthly plan with a 3-day free trial.

Yearly plan with no free trial.

Decide on the payment provider (Stripe, RevenueCat, or custom in-app purchase flow).

Plan Selection Screen

Let users pick either the monthly or yearly plan.

Include a 3-day trial for the monthly plan only.

On successful payment, mark the user in the database with an active subscription.

Subscription Validation

Validate subscriptions on each app start (e.g., by checking your payment provider’s APIs).

Show subscription screens or reminders if the user’s subscription is inactive or expired.

5. Onboarding Flow
Relationship Type

Screen to choose if it’s monogamous or if there are more partners.

Gender Selection

Two emojis (eggplant and peach) for quick selection.

Kinks and Common Fetishes

Checkboxes or a list for users to pick from.

Store these preferences in Supabase (e.g., profiles table).

Secret Kinks

Optional screen where a user can list private interests.

Explain that these will be suggested to the partner in a discreet way.

Connection with Partner(s)

Generate or enter a code to link accounts.

Match up user profiles in the DB.

6. Primary Flow: Ring or Request
Home Screen with “Ring” Button

Tapping it triggers a notification to the partner(s).

Create a record in requests table to mark the ring.

Receiving Side

Display an alert or push notification that a session was requested.

Include “Accept” or “Decline” buttons.

7. Coin Toss
Start Coin Toss

If the partner accepts, run a quick coin flip animation.

Decide who goes first (sender or receiver).

Display Result

Show a simple heads/tails outcome to both users.

8. Instruction and Guides
Initial Content Source

Have a small, hardcoded list of suggestions in the app or in a Supabase table (e.g., suggestions).

Filter or randomize the suggestions based on the user’s marked interests.

Display Chosen Suggestion

Show the user a step or tip for foreplay or a bigger challenge.

Marking Completion

After the suggestion is done, the user can tap “Complete.”

9. Completion and Sharing
Post-Session Screen

Offer a brief note: “Session completed! Want to share a discreet update?”

Social Sharing

Provide a button that shares a simple message: “We just finished a playful challenge!” with a referral link.

Make sure details remain private.

Referral Tracking

If a shared link is used by someone else, grant rewards to the referrer.

10. Notifications
Push Notifications Setup

Use Expo’s push notification system.

Register tokens and store them in your profiles table.

When “Ring” is sent, call the Expo notification endpoint to alert the partner(s).

11. Badges and Challenges
Simple Badge System

Award a badge for first session, 5 sessions, 10 sessions, etc.

Store badge data in the DB, display it in the user’s profile.

Challenge Ideas

Could be timed or progressive (e.g., “Try 3 different suggestions this week”).

12. Analytics (Basic Version)
Session Counts

Log each session in the DB, track how often users open the app or complete suggestions.

Coin Toss Stats

Keep track of total heads vs. tails for fun.

Favorite Categories

If a user marks certain suggestions as favorites, store that data to guide future suggestions.

Summary

Show a mini-dashboard with usage info (like total sessions, favorite suggestion types, etc.).

13. Feedback Loop
User Feedback on Suggestions

Thumbs up/down or a quick rating.

Use this to refine suggestions over time.

Notes Section

Let users jot down what went well or what didn’t.

Store these privately in the DB.

14. Final Steps / Polish
Testing

Run on both iOS and Android devices (or simulators).

Make sure push notifications, ring requests, and subscription flows work end to end.

Publishing

Prepare your app for release on Google Play and App Store.

Set up the subscription items and free trials correctly in each store.

Ongoing Maintenance

Monitor user feedback.

Tweak suggestions and fix bugs as they come up.

