# FileMaker OData v4 API - Postman Collection

A comprehensive Postman collection for working with FileMaker Server's OData v4 API.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://god.gw.postman.com/run-collection/YOUR_COLLECTION_ID)

## Author

**Doug Hauenstein**  
FM Rug Software  
🌐 [fmrug.com](https://fmrug.com) | [nectomax.com](https://nectomax.com)

---

## What's Included

This collection contains **40+ requests** organized into logical folders:

| Folder | Description |
|--------|-------------|
| 🔍 **Metadata & Discovery** | Service document, schema exploration |
| 📄 **CRUD Operations** | Create, Read, Update, Delete records |
| 🔎 **Query Options** | $filter, $select, $orderby, $top, $skip, $count |
| 🔗 **Related Records** | $expand, navigation properties |
| 📎 **Container Fields** | Upload, download, manage files |
| 📦 **Batch Operations** | Multiple operations in one request |
| ⚙️ **FileMaker Extensions** | Global fields, script execution |
| ❌ **Error Examples** | Understanding error responses |

---

## Quick Start

### 1. Import into Postman

**Option A: Download and Import**
1. Download both JSON files from this repo
2. In Postman, click **Import** (top left)
3. Drag both files or click "Upload Files"

**Option B: Run in Postman Button**
Click the "Run in Postman" button above to fork directly into your workspace.

### 2. Configure Your Environment

After importing, set these environment variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `baseUrl` | Your FileMaker Server URL | `https://fm.example.com` |
| `database` | Database name | `MyDatabase` |
| `table` | Default table/layout | `Customers` |
| `recordId` | Default record ID | `1` |
| `username` | FileMaker account | `api_user` |
| `password` | Account password | `••••••••` |

### 3. FileMaker Server Setup

**Enable OData:**
1. Open FileMaker Server Admin Console
2. Go to **Connectors** → **OData**
3. Enable OData access

**Configure Privileges:**
Your FileMaker account needs:
- `fmrest` extended privilege enabled
- Appropriate record-level access for exposed layouts

---

## Files

```
├── FileMaker_OData_v4_API.postman_collection.json    # Main collection
├── FileMaker_OData_v4.postman_environment.json       # Environment template
└── README.md                                          # This file
```

---

## Key Features

### Complete Query Options Coverage

```
# Filter active customers with orders over $500
GET /Customers?$filter=Status eq 'Active' and TotalOrders gt 500

# Select specific fields, sorted, with pagination
GET /Customers?$select=Name,Email&$orderby=Name&$top=25&$skip=50&$count=true

# Include related records
GET /Customers?$expand=Orders($filter=Total gt 100;$orderby=OrderDate desc)
```

### FileMaker-Specific Extensions

**Execute scripts via headers:**
```
X-FM-Script-Name: MyScript
X-FM-Script-Param: {"action":"process"}
```

**Set global fields:**
```json
{
    "globalFields": {
        "Globals::g_UserID": "12345"
    }
}
```

### Container Field Operations

- **Download:** `GET /Table(1)/Photo/$value`
- **Upload:** `POST /Table(1)/Document/$value` with binary body
- **Clear:** `DELETE /Table(1)/Photo/$value`

---

## Important Notes

### JSON Escaping for POST/PATCH

When sending requests programmatically (webhooks, scripts, etc.), FileMaker OData v4 requires escaped quotes within string values:

```javascript
// In code, escape the JSON properly
const body = JSON.stringify({
    "Notes": "Customer said \"call back later\""
});
// Results in: {"Notes":"Customer said \"call back later\""}
```

Postman handles this automatically, but it's critical for Pipedream, n8n, or custom integrations.

### Field Names Are Case-Sensitive

OData field names must match your FileMaker field names exactly:
- ✅ `FirstName` 
- ❌ `firstname`
- ❌ `FIRSTNAME`

### Layouts Define Your Schema

OData exposes **layouts**, not tables directly. The fields available via OData are determined by which fields exist on your layout.

---

## Common Patterns

### Pagination

```javascript
// Page size: 25 records
// Formula: $skip = (pageNumber - 1) × pageSize

Page 1: ?$top=25&$skip=0
Page 2: ?$top=25&$skip=25
Page 3: ?$top=25&$skip=50
```

### Dashboard Query (Combined Options)

```
GET /Customers
    ?$select=CustomerID,Name,Email,TotalOrders
    &$filter=Status eq 'Active' and TotalOrders gt 5
    &$orderby=TotalOrders desc
    &$top=20
    &$count=true
```

### Get Count Only

```
GET /Customers/$count?$filter=Status eq 'Active'
```
Returns: `237` (plain integer)

---

## Resources

- [Claris OData Guide](https://help.claris.com/en/odata-guide/)
- [OData v4 Specification](https://www.odata.org/documentation/)
- [FM Rug Software](https://fmrug.com) - FileMaker SaaS for carpet & rug cleaning businesses

---

## Contributing

Found an issue? Have an improvement? 

- Open an issue
- Submit a PR
- Reach out via [fmrug.com](https://fmrug.com)

---

## License

MIT License - Use it, modify it, share it.

Free for the FileMaker community. 🎉

---

*Last updated: January 2025*
