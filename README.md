# Blink Bihar - CSV Import & Setup Guide

This guide explains how to import vendors and products into Blink Bihar using CSV files.

## Setup

### 1. Environment Variables

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Required variables:
- `MONGO_URI` or `DATABASE_URL`: MongoDB connection string
- `JWT_SECRET`: Secret key for JWT authentication
- `RAZORPAY_KEY_ID` & `RAZORPAY_KEY_SECRET`: Razorpay credentials for payments
- `PORT`: Server port (default: 5000)

### 2. Image Asset Directories

Upload your vendor and product images to:

```
client/public/vendor-logos/     # Vendor logo images
client/public/vendor-banners/   # Vendor banner/cover images  
client/public/products/         # Product images
```

File names in your CSV should match the actual filenames in these directories.
If an image file is not found, a placeholder will be used automatically.

## CSV Import Methods

### Method 1: Admin UI (Recommended)

1. Login as an admin user
2. Navigate to **Admin Console → Data Import**
3. Download CSV templates for vendors and products
4. Fill in your data following the template format
5. Upload and validate your CSV (dry-run check)
6. If validation passes, click "Import" to commit the data

Benefits:
- Visual feedback and error reporting
- Download error CSV if any rows fail
- No command line required

### Method 2: CLI Scripts

For bulk imports or automation:

```bash
# Import vendors
npm run import:vendors ./data/vendors.csv

# Import products  
npm run import:products ./data/products.csv
```

Place your CSV files in the `data/` directory for easy access.

## CSV Schemas

### Vendors CSV

**Required columns** (must be exact, case-sensitive):
```csv
vendorId,shopName,ownerName,phone,whatsapp,email,addressLine1,addressLine2,city,state,pincode,lat,lng,serviceRadiusKm,categories,openingHours,closedDays,logoFileName,bannerFileName,payoutUPI,gstin,minOrderAmount,deliveryFeeBase
```

**Field details:**
- `vendorId`: Unique identifier (string)
- `shopName`: Store display name
- `ownerName`: Owner's full name
- `phone`: Contact number (required)
- `whatsapp`: WhatsApp number (optional)
- `email`: Email address (optional)
- `addressLine1`: Primary address line
- `addressLine2`: Secondary address line (optional)
- `city`: City name
- `state`: State name
- `pincode`: Postal code
- `lat`: Latitude (decimal, optional - can be set later via Geocode tool)
- `lng`: Longitude (decimal, optional)
- `serviceRadiusKm`: Delivery radius in km (default: 3)
- `categories`: Semicolon-separated list (e.g., "grocery;vegetables;dairy")
- `openingHours`: Format "HH:MM-HH:MM" (e.g., "08:00-22:00")
- `closedDays`: Semicolon-separated days (e.g., "Sunday;Monday") (optional)
- `logoFileName`: Filename in `/client/public/vendor-logos/` (optional)
- `bannerFileName`: Filename in `/client/public/vendor-banners/` (optional)
- `payoutUPI`: UPI ID for payouts (optional)
- `gstin`: GST number (optional)
- `minOrderAmount`: Minimum order value (default: 199)
- `deliveryFeeBase`: Base delivery fee (default: 15)

**Upsert behavior:**
- If `vendorId` exists: Updates the vendor
- If new: Creates a new vendor

### Products CSV

**Required columns**:
```csv
sku,vendorId,name,description,mrp,price,unit,stock,category,subcategory,imageFileName,tags,isActive
```

**Field details:**
- `sku`: Unique product identifier (string)
- `vendorId`: Must match an existing vendor's `vendorId`
- `name`: Product name
- `description`: Product description (optional)
- `mrp`: Maximum retail price (number)
- `price`: Selling price (number)
- `unit`: Unit description (e.g., "500 g", "1 L")
- `stock`: Available quantity (integer)
- `category`: Product category
- `subcategory`: Product subcategory (optional)
- `imageFileName`: Filename in `/client/public/products/` (optional)
- `tags`: Semicolon-separated tags (e.g., "organic;fresh") (optional)
- `isActive`: "true" or "false" (default: true)

**Upsert behavior:**
- If `sku` exists: Updates the product
- If new: Creates a new product

## Geocoding Vendors

If you don't have lat/lng coordinates:

1. Import vendors without coordinates (leave `lat` and `lng` empty)
2. Navigate to **Admin Console → Geocode Vendors**
3. Select a vendor from the list
4. Manually enter coordinates or use Google Maps to find them
5. Click "Save Coordinates"

The vendor's `geocodeStatus` will change from "pending" to "ok".

## Proximity Search

Once vendors are geocoded:

1. Users set their delivery location on the home page
2. The app uses MongoDB's 2dsphere index to find nearby vendors
3. Only vendors within:
   - User's search radius (default 10km), AND
   - Vendor's own `serviceRadiusKm`
   ...will be shown

This ensures accurate, location-based vendor discovery.

## Validation & Error Handling

Both import methods validate every row:

- **Field types**: Ensures numbers are numeric, required fields are present
- **Vendor existence**: Products validate that their `vendorId` exists
- **Image files**: Checks if referenced files exist in asset directories
- **CSV format**: Validates proper CSV structure

If errors occur:
- **Admin UI**: Shows error count and allows downloading an error CSV with details
- **CLI**: Prints errors to console with row numbers and specific issues

Fix the errors in your source CSV and re-import.

## Example Workflow

1. **Prepare CSVs:**
   - Download templates from Admin UI
   - Fill in vendor and product data
   - Upload images to respective directories

2. **Import vendors first:**
   - Admin UI: Upload vendors.csv → Validate → Import
   - CLI: `npm run import:vendors ./data/vendors.csv`

3. **Geocode vendors:**
   - Admin → Geocode Vendors
   - Set coordinates for each vendor

4. **Import products:**
   - Admin UI: Upload products.csv → Validate → Import  
   - CLI: `npm run import:products ./data/products.csv`

5. **Verify:**
   - Check home page to see vendors appear
   - Search by location to test proximity filtering
   - Browse products within vendor catalogs

## Troubleshooting

**"Vendor not found" error when importing products:**
- Ensure vendors are imported first
- Check that `vendorId` in products CSV exactly matches vendor's `vendorId`

**Images not showing:**
- Verify filenames in CSV match actual files (case-sensitive)
- Check files are in correct directories
- Restart server to reload public assets

**Geocoding issues:**
- Coordinates should be decimal format (e.g., 25.5941, 85.1376)
- Latitude ranges: -90 to 90
- Longitude ranges: -180 to 180

**MongoDB connection errors:**
- Verify `MONGO_URI` in `.env` is correct
- Ensure MongoDB is running and accessible
- Check network/firewall settings

## Notes

- CSV headers must match exactly (case-sensitive)
- Semicolons (`;`) are used to separate multiple values in a field
- Empty optional fields can be left blank
- Images default to placeholders if files not found
- Upsert behavior allows re-importing to update existing records

For more help, check server logs or contact your system administrator.
