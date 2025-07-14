# 📅 Project Requirement Analysis – July 14, 2025

## 🔗 URL Shortener Project Analysis

This project involves building a URL shortener service that allows registered users to create and manage short links.

### ✅ Functional Requirements

- **User Authentication**  
  Each user should be able to manage their own links, requiring a secure authentication system (e.g., email/password or OAuth).

- **URL-Shortening API/Endpoint**  
  A core API endpoint for shortening long URLs is essential.

- **Link Redirection**  
  Short links must accurately redirect to their original long URLs.

- **Link Management**  
  Users need a dashboard to view, create, edit, and delete their short links.

- **Custom Aliases (Slugs)**  
  Users should have the option to define custom short codes (e.g., `site.com/Promo2025`), a common feature in self-hosted shorteners.

- **Analytics**  
  The system must track detailed statistics for each link click, including timestamp, user IP, device/OS, browser, referrer, and geographic location.  
  - Aggregated statistics such as total clicks over time, browser/OS breakdown, and country-specific data should be viewable.  
  - Leading services provide click counts, geolocation, and referrer data.  
  - Visit stats should collect anonymized data with geo and browser information.

- **Optional Features**  
  Support for link expiration dates or maximum visit caps per link.

### ⚙️ Technical Considerations & Architecture

- **Backend**  
  Use Node.js (e.g., Express) with PostgreSQL for storing users, links, and visit logs.

  **Schemas (Can be changed)**:
  - `users`: `id`, `email`, `password_hash`, `created_at`
  - `short_links`: `id`, `user_id`, `slug`, `original_url`, `created_at`, `expires_at`, `max_visits`, `is_active`
  - `visits`: `id`, `short_link_id`, `timestamp`, `ip`, `country`, `city`, `user_agent`, `referer`

  Each link access should increment a counter and insert a visit record for analytics.

- **Frontend**  
  A simple HTML/CSS/JS frontend with REST API calls for login, link creation, and statistics. Server-side templating or a lightweight JS framework (such as EJS) may be used.

- **Rate Limiting and Security**
  - Implement rate limiting on APIs to prevent abuse (e.g., 100 requests per 15 minutes per IP).
  - Use security middleware (e.g., Helmet) and validate all input URLs.
  - Accept only well-formed URLs with `http` or `https`.
  - Enforce HTTPS in production.

- **Logging**  
  Log all requests using tools like Morgan or Winston for auditing.

- **Deployment (Can be changed)**  
  Containerize with Docker and deploy to a cloud provider. Use environment variables for secrets and configs.

- **Analytics & Scaling**
  - Store detailed stats: click timestamp, country, browser/OS, referrer.
  - Display aggregated charts: clicks per day, top countries, referrers.
  - Retain historical data for trend analysis.
  - Prune or archive old logs if necessary.
  - Use indexes (e.g., on `slug`) and caching (e.g., LRU) for performance.

### 💡 Inspirations

- **YOURLS**: Known for historical stats, referrer tracking, and geo-location.
- **Shlink**: Features custom slugs, multi-domain support, and detailed visit stats.
- **Bitly**: Offers real-time analytics and long-term history.

---

## 🏨 Rate Shopping Web Scraper Project Analysis

This project aims to build a scraper that collects hotel pricing and availability from various travel websites, especially those covering Turkey.

### ✅ Functional Requirements

- **Data Fetching**  
  Given a search query (city or coordinates, dates, occupancy), the scraper should fetch hotel listings and prices from sources like Booking.com, Hotels.com, Expedia, Trivago, Otelz, and Enuygun.

- **Data Aggregation and Comparison**  
  Match the same hotel across different sources and record the price variations.

- **Data Storage**  
  Store the collected data in PostgreSQL for later analysis.

- **Target Sites**  
  Scrape both global and Turkish travel sites to ensure wide coverage and find the best rates.

### ⚙️ Technical Considerations & Architecture

- **Scraping Approach**  
  Use Node.js with:
  - **Puppeteer**: For dynamically loaded content.
  - **Cheerio**: For static HTML parsing (with Axios or fetch).

- **Rate Limiting and Proxies**
  - Rotate user agents and IP addresses.
  - Add random delays to mimic human behavior.
  - Respect `robots.txt` where applicable.

- **Data Extraction**
  Extract hotel details like name, address, city, rating, price, currency, and amenities.

- **Handling Dynamic Prices**
  Handle XHR requests for price data or allow full page render in Puppeteer.

- **Scheduling**
  Use `node-cron` or cloud schedulers to run scraping jobs (hourly/daily). Log job start/end times and retry failed tasks.

- **Data Model (PostgreSQL) (Can be changed)**:
  - `hotels`: `id`, `name`, `address`, `city`, `country`, `latitude`, `longitude`
  - `sites`: `id`, `name`, `base_url`
  - `hotel_offers`: `id`, `hotel_id`, `site_id`, `checkin`, `checkout`, `currency`, `price`, `scraped_at`

  Match or insert hotel entries for comparison and track historical pricing.

- **Output & Usage**
  Initial output can be JSON or CSV.  
  Support queries or a simple API to retrieve cheapest prices or price history charts.

- **Best Practices**
  - Add error handling and logs.
  - Monitor for bans and rotate IPs.
  - Alert on recurring failures.
  - Respect legal and ethical boundaries.

- **Deployment**
  Containerize the scraper (e.g., Docker) and run scheduled tasks. Use environment variables for site configs and credentials. Future improvements may include a web dashboard.

 ### 💡 Inspirations

- **Trivago**: Aggregates hotel prices and highlights the cheapest.
- **Kayak**: Combines hundreds of OTAs in one search.
- **Enuygun**: Offers local deals and discounts.
