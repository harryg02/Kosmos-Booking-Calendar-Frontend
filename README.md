# Kosmos Stargazing Resort - Custom Mews Availability Calendar - Frontend

This project enhances the booking experience for the Kosmos Resort website by providing a custom, real-time availability calendar before redirecting users to the Mews booking engine.

It consists of two main parts:
1.  **Backend:** A secure serverless function that communicates with the Mews Connector API.
2.  **Frontend:** Custom JavaScript and CSS embedded within a Webflow site to display the interactive calendar.

This system allows guests to select their desired villa on the website and immediately see a calendar showing only the available dates for that specific villa, significantly improving user confidence and reducing booking friction.

## Architecture

*   **Frontend:** The user interface is built and hosted on **Webflow**. It uses custom code embeds for CSS styling and JavaScript logic.
    *   **Repository:** The source code for the frontend function is located at: [harryg02/Kosmos-Booking_Calendar-Frontend](https://github.com/harryg02/Kosmos-Booking-Calendar-Frontend)
    *   **UI Library:** [Flatpickr.js](https://flatpickr.js.org/) is used to render the interactive calendar.
    *   **Logic:** Custom Vanilla JavaScript handles user interactions (opening the calendar), calling the backend API, processing availability data, and constructing the final Mews deeplink.

*   **Backend:** A Node.js serverless function is deployed on **Vercel** to act as a secure proxy.
    *   **Repository:** The source code for the backend function is located at: [kosmosharry/mews-availability.js](https://github.com/kosmosharry/mews-availability.js)
    *   **Function:** This function receives requests from the Webflow frontend, securely calls the Mews Connector API (`/api/connector/v1/services/getAvailability`) with protected credentials, processes the availability data, and returns a simplified list of unavailable dates to the frontend.

## Features

*   **Real-Time Availability:** Fetches live availability data from the Mews Connector API.
*   **Villa-Specific Calendar:** Displays a calendar filtered to show availability for only the specific villa the guest has selected.
*   **Minimum Stay Logic:** Intelligently disables dates that can only be used as a check-out date, preventing users from selecting invalid stay ranges.
*   **Infinite Scroll:** Automatically fetches availability for future months as the user navigates the calendar.
*   **Responsive Design:** Calendar layout adjusts from two months to a single month on smaller screens.
*   **Dynamic Deeplinking:** Constructs a complete Mews Distributor URL with the selected villa, dates, and optional promo code pre-filled for a seamless transition to the final booking steps.

## Frontend Setup (Webflow)

The following code is intended to be placed within the **Custom Code** sections of the Webflow project.

### 1. Head Code

Place the following CSS styles in **Project Settings > Custom Code > Head Code**. This code handles all the custom styling for the Flatpickr calendar, including selected states, range highlighting, loading indicators, and responsiveness.

```html
<style>
  /* Flatpickr custom styles */
  .flatpickr-calendar {
    box-shadow: none !important;
  }
  .flatpickr-day.selected,
  .flatpickr-day.startRange,
  /* ... (all other CSS rules from your code) ... */
  .loading-overlay .flatpickr-calendar {
    display: none !important;
  }
</style>```

### 2. Footer Code (Before `</body>` tag)

Place the following JavaScript in **Project Settings > Custom Code > Footer Code**. This is the core logic that powers the calendar.

**Important:** You must include the Flatpickr library *before* this custom script.

```html
<!-- Include Flatpickr JS Library first -->
<script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>

<!-- Custom Calendar Logic -->
<script>
  // --- Helper Function to format Date to YYYY-MM-DD ---
  function formatDate(date) {
    // ... (all other JavaScript code from your file) ...
  }
  
  // ... (rest of the script) ...

  // Add event listeners to the specific buttons
  document.addEventListener('DOMContentLoaded', function() {
    // ... (DOM event listeners) ...
  });
</script>
```

### 3. Webflow Designer Setup

*   **Modal Pop-ups:** Create modal/pop-up elements for each villa's calendar.
*   **Trigger Buttons:** Ensure the "Book" buttons for each villa have the specific IDs used in the `DOMContentLoaded` event listener (e.g., `Book-Galaxy-Villa`, `Book-Stargazing-Villa`).
*   **Calendar Container:** Inside each modal, place a Code Embed element containing a `<div>` with the appropriate ID (e.g., `myCalendarContainer` or `myCalendarContainer2`) and a `data-villa-id` attribute containing the Mews Space Category ID for that villa.
    ```html
    <!-- For Galaxy Villa Modal -->
    <div id="myCalendarContainer" data-villa-id="25e8c786-0920-46f0-bf62-b1c4007930c5"></div>
    ```
*   **Next Button & Promo Code:** Place the "Next Step" button and the optional promo code input field within the modal. Ensure the button has the correct ID (e.g., `calendar-next-btn` or `calendar-next-btn2`).

## Backend Setup (Vercel)

The backend is a Node.js serverless function. For full setup instructions, refer to the repository: [https://github.com/kosmosharry/mews-availability.js](https://github.com/kosmosharry/mews-availability.js)

### Environment Variables

The Vercel deployment requires the following environment variables to connect to the Mews Connector API:
*   `MEWS_CLIENT_TOKEN`
*   `MEWS_ACCESS_TOKEN`
*   `MEWS_SERVICE_ID`
*   `MEWS_CONNECTOR_API_URL`
