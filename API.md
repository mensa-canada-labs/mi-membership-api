# Mensa International Membership API - Proposal

**⚠️ IMPORTANT: This is a technical specification proposal and not an official Mensa International project.**

This document presents a proposed API system that would enable national Mensa organizations to **synchronize their member data** with Mensa International systems. This is a **technical specification proposal** designed to demonstrate how technology could streamline data sharing and coordination across the global Mensa network.

## Table of Contents

### Part I - Non-Technical Overview (Business Focus)
1. [Why This API Is Needed](#why-this-api-is-needed)
2. [Executive Summary](#executive-summary)
3. [Key Terms](#key-terms)
4. [Business Benefits](#business-benefits)
5. [Implementation Overview](#implementation-overview)

### Part II - Technical Specifications (Development Focus)
1. [API Overview](#api-overview)
2. [Endpoints](#endpoints)
3. [Error Handling](#error-handling)
4. [Monitoring and Observability](#monitoring-and-observability)
5. [Status Codes](#status-codes)
6. [Data Validation Rules](#data-validation-rules)
7. [Rate Limiting](#rate-limiting)
8. [Best Practices](#best-practices)

---

# PART I: NON-TECHNICAL OVERVIEW

*This section is designed for managers, administrators, and non-technical stakeholders to understand the business value, implementation requirements, and operational impact of the proposed API system.*

## Why This API Is Needed

Currently, national Mensa organizations manage their members using various independent systems with limited data sharing with Mensa International. Members need access to Mensa International's member-only services and systems. This creates several operational challenges:

### Current Challenges
- **Manual Data Sharing**: National Mensas must manually provide member lists to Mensa International for system access
- **Delayed Access**: Members may experience delays accessing Mensa International services after joining or renewing
- **Administrative Burden**: Maintaining separate member lists requires duplicate data entry and updates
- **Transfer Complexity**: Member transfers between countries require extensive manual coordination between systems
- **Inconsistent Data**: Member information may become outdated between national and international systems

### Proposed Solution Benefits
- **Automated Data Synchronization**: National systems automatically share member updates with Mensa International
- **Immediate Member Access**: Members gain instant access to Mensa International services upon joining/renewing
- **Streamlined Transfers**: Automated coordination when members relocate between countries
- **Consistent Global Data**: Member information stays synchronized across national and international systems

## Executive Summary

The proposed Mensa International Members API would serve as a **data synchronization bridge** between national Mensa systems and Mensa International systems. This system would enable:

- **Automated Data Sharing**: Member updates automatically sync with Mensa International
- **Immediate Service Access**: Members gain instant access to Mensa International member services
- **Simplified Transfers**: When members relocate, their information transfers seamlessly between countries
- **Consistent Global Data**: Member information stays synchronized without duplicating management effort

## Key Terms

**National Mensa**: A country-specific Mensa organization (e.g., Mensa US, Mensa Germany, Mensa Japan).

**Mensa International (MI)**: The umbrella organization that provides member services and coordinates between national Mensa associations worldwide.

**Data Synchronization**: Sharing member information between national Mensa systems and Mensa International systems to keep records consistent.

**Member Transfer**: Coordinating member information when someone relocates from one national Mensa to another.

**API (Application Programming Interface)**: A system that allows different computer programs to communicate with each other automatically.

### Key Data Sharing Operations

The system would handle sharing essential membership information with Mensa International:

- **New Member Notifications**: Inform Mensa International when new members join the national Mensa
- **Member Transfer Coordination**: Coordinate with Mensa International when members relocate between countries  
- **Membership Status Updates**: Share renewal information and membership status changes
- **Member Departure Notifications**: Update Mensa International when members leave or don't renew
- **Member Information Updates**: Sync contact information changes and administrative updates
- **Traveler Verification**: Allow other national Mensas to verify members for international events

### How It Works

**System Integration**: Each national Mensa connects their membership system to the API using secure credentials provided by Mensa International.

**Flexible Synchronization**: When national Mensas process membership operations in their current systems (new members, renewals, updates), they can choose to notify Mensa International through the API to keep records synchronized according to their own schedule and requirements.

**Efficient Batch Processing**: Instead of sending updates one by one, member information can be synchronized in groups (up to 500 at once), making large operations like annual renewals much more efficient.

**Independent Processing**: If one member's information fails to sync (due to data formatting issues), other members in the same batch are still processed successfully.

**Transfer Coordination**: When a member relocates, the API facilitates data sharing between the source and destination national Mensa organizations and Mensa International.

**System Autonomy**: Each national Mensa retains complete control over membership policies, fees, processes, member management, and synchronization timing. The API only handles data sharing coordination when requested.

### Security and Reliability

**Secure Access**: Each national Mensa receives unique, secure access credentials that only allow them to manage their own members. No national Mensa can access another organization's member data.

**Usage Limits**: Built-in safeguards prevent system abuse with reasonable limits on API usage (suggested: 1000 operations per hour for regular tasks, 30 per hour for member verification).

**Data Protection**: All data transmission is encrypted and validated to ensure member information remains secure and accurate.

**Complete Audit Trail**: Every operation is logged with unique tracking identifiers for accountability and troubleshooting.

### Business Benefits

**Operational Independence**: National Mensas maintain complete autonomy over their membership policies and processes while enabling better coordination.

**Reduced Administrative Work**: Automated data synchronization eliminates manual reporting to Mensa International and reduces duplicate data entry.

**Enhanced Member Experience**: Members gain immediate access to Mensa International services upon joining or renewing through their national organization.

**Simplified Transfers**: Members relocating between countries experience seamless coordination between their old and new national Mensa.

**Consistent Global Data**: Member information stays synchronized between national and international systems without extra effort.

**Cost Efficiency**: Reduced manual coordination and fewer data discrepancies lead to lower administrative costs for all organizations.

## Implementation Overview

### What National Mensas Need to Do

**Simple Integration**: Connect the membership management system to the API. No changes to current membership processes are required.

**Add Data Synchronization**: Set up the system to share member information with Mensa International when desired. This is typically a small addition to current workflows.

**Configure Sync Operations**: Set up the system to send member updates in efficient batches (groups of 50-200 work best), with basic error handling for any synchronization issues.

**Monitor and Support**: Use the built-in tracking system to monitor synchronization status and access dedicated support channels for technical assistance.

---

*The business overview above provides sufficient information for non-technical stakeholders to understand the system's purpose, benefits, and implementation approach.*

**→ Non-technical readers can stop here - they have all the information needed to understand the proposal.**

---

# PART II: TECHNICAL SPECIFICATIONS

*This section provides complete technical documentation for development teams implementing the API integration.*

**← [Back to Business Overview](#part-i-non-technical-overview)**

## Technical Glossary

### API Technical Terms

**Batch Operation**: A single API request that processes multiple member operations together, improving efficiency for large datasets.

**Partial Success Model**: An approach where individual operations within a batch can succeed or fail independently, eliminating the need to reprocess entire batches due to individual errors.

**API Key**: A unique authentication token assigned to each national Mensa organization that serves both for secure access to the API and to identify which national Mensa is making the request. All operations are automatically scoped to the organization associated with this key.

**Request ID**: A unique identifier assigned to each API request for tracking, debugging, and support purposes (format: `req_abc123def456`).

**Rate Limiting**: Security mechanism that restricts the number of API requests allowed per time period to prevent abuse and ensure system stability.

**Burst Protection**: Additional rate limiting that prevents sudden spikes in request volume beyond normal operational patterns.

**Idempotency**: The property that allows the same API request to be safely repeated multiple times without causing unintended side effects.

**Correlation ID**: An optional client-provided identifier that allows end-to-end tracking of requests across multiple systems.

### Data Format Terms

**ISO 3166-1 alpha-2**: The international standard for two-letter country codes.

**ISO 8601**: The international standard for date and time formatting used throughout the API (e.g., `"2025-01-15T00:00:00.000Z"`).

**National Member ID**: The unique identifier assigned to a member by their national Mensa organization.

### Technical Infrastructure Terms

**RESTful API**: An architectural style for web APIs that uses standard HTTP methods and follows REST principles.

**HTTPS**: Secure HTTP protocol that encrypts data transmission between clients and the API server.

**CORS (Cross-Origin Resource Sharing)**: A security feature that controls how web applications from different domains can access the API.

**JSON (JavaScript Object Notation)**: The data format used for all API requests and responses.

**Content Security Policy (CSP)**: Security headers that help prevent cross-site scripting and other code injection attacks.

---

*The sections below provide detailed technical information for implementation teams.*

## API Overview

### API Versioning

This API uses version prefixing in the URL path (`/v1/`) to ensure backward compatibility. The current version is `v1` and is designed to remain stable for national Mensa associations, allowing internal Mensa International systems to evolve independently.

### Authentication

All API requests require authentication via API key. The API key serves a dual purpose: it authenticates the request and identifies which national Mensa organization is making the changes. This identification is crucial as it determines which members can be accessed and modified.

#### API Key Authentication

- **Header**: `X-API-Key`
- **Format**: String value provided by Mensa International
- **Scope**: Each national Mensa association has a unique API key that scopes all operations to their organization only

**Note**: API key management (distribution, rotation, revocation, and lifecycle management) is handled through separate administrative processes and is not covered in this technical specification.

**Example**:

```http
X-API-Key: ca_abc123def456ghi789jkl012mno345pqr678stu901vwx234yz567
```

#### Authentication Flow

1. National Mensa receives API key from Mensa International
2. Include API key in `X-API-Key` header for all requests
3. API validates key against database and checks if national Mensa is active
4. **API identifies the requesting national Mensa organization from the key**
5. All operations are automatically scoped to the authenticated national Mensa's members only

### Security Headers

The API includes security headers in all responses to protect against common web vulnerabilities, especially when accessed via web interfaces hosted on Mensa domains.

#### Standard Security Headers

All API responses include the following security headers:

```http
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Cache-Control: no-store, no-cache, must-revalidate, private
```

#### Content Security Policy

For web-based access from Mensa domains:

```http
Content-Security-Policy: default-src 'self' *.mensa.org; \
  connect-src 'self' api.mensa.org; \
  script-src 'self' 'unsafe-inline' *.mensa.org; \
  style-src 'self' 'unsafe-inline' *.mensa.org
```

#### HTTPS Enforcement

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

#### CORS Configuration

CORS is configured to allow access from Mensa International domains:

```http
Access-Control-Allow-Origin: *.mensa.org
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: X-API-Key, Content-Type, Authorization
Access-Control-Max-Age: 3600
```

**Note**: Server-to-server integrations are not affected by CORS policies.

## Endpoints

### Health Check

#### GET /v1/health

Health check endpoint to verify API status. Requires authentication for security purposes.

**Request**:

```http
GET /v1/health HTTP/1.1
Host: api.mensa.org
X-API-Key: your-api-key-here
```

**Response**:

```json
{
  "status": "ok",
  "timestamp": "2025-01-01T12:00:00.000Z",
  "service": "mensa-international-membership-api",
  "version": "1.0.0"
}
```

**Response Headers**:

```http
X-Request-ID: req_abc123def456
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Cache-Control: no-store, no-cache, must-revalidate, private
```

### Member Operations (Batch)

#### POST /v1/members/batch

Process a batch of member operations for the national Mensa organization. This endpoint handles all member lifecycle operations (join, renew, leave, exclude, update) with individual operation-level error handling.

**Key Features**:
- **Individual Operation Processing**: Each member operation is processed independently
- **Partial Success Support**: Individual operations can fail while others succeed
- **Rate Limited**: Suggested limits: 1000 requests/hour + 100 requests/minute burst protection

**Request**:

```http
POST /v1/members/batch HTTP/1.1
Host: api.mensa.org
Content-Type: application/json
X-API-Key: your-api-key-here
X-Request-ID: batch_sync_2025_07_22_001
User-Agent: MensaCA-Integration/2.1.0

[...batch operations...]
```

**Request Headers**:
- `Content-Type: application/json` - **Required**
- `X-API-Key` - **Required**: National Mensa API key
- `X-Request-ID` - **Optional**: Client correlation ID for tracing
- `User-Agent` - **Recommended**: Descriptive client identifier

**Request Constraints**:
- Maximum 500 operations per batch (recommended)
- Maximum request size: 10MB
- Request timeout: 30 seconds

The batch endpoint accepts an array of operation objects. Each operation specifies an action and associated data. Operations are processed in the order provided, with each action group handled atomically.

#### Operation Structure

```json
{
  "action": "string",
  "data": []
}
```

**Supported action values**: `"join"`, `"renew"`, `"leave"`, `"exclude"`, `"update"`

**Field descriptions**:
- `action`: The type of operation to perform on the members
- `data`: Array of operation-specific data objects

#### Supported Actions

| Action | Purpose | Processing Model | Typical Use Cases |
|--------|---------|------------------|-------------------|
| `join` | Add new members, transfers, or returning members | Per member | New registrations, transfers from other national Mensa, members returning after >12 months |
| `renew` | Extend membership periods for active/recently expired members | Per member | Annual renewals, late renewals within 12 months |
| `leave` | Process member departures | Per member | Non-renewals, transfers to other national Mensa |
| `exclude` | Disciplinary exclusions | Per member | Conduct violations, policy breaches |
| `update` | Modify member information | Per member | Contact updates, name changes |

#### Response Format

All batch operations return a consistent response structure:

```json
{
  "results": [
    {
      "action": "string",
      "success": "boolean",
      "result": "array or object"
    }
  ]
}
```

**Field Descriptions**:
- `action`: The action that was processed
- `success`: Whether this action succeeded (true/false)
- `result`: Action-specific result data or error details

**Response Structure**:
- Check individual `results[].success` values for operation status
- Request IDs available in response headers for debugging
- Individual operations can succeed while others fail within the same request

#### Performance and Security Considerations

##### Batch Size Optimization
- **Recommended batch size**: 50-200 operations for optimal performance
- **Maximum batch size**: 500 operations (hard limit)
- **Large datasets**: Split into multiple batches with appropriate delays

##### Processing Model
- **Individual member processing**: Each member operation within a batch is processed independently
- **Best-effort execution**: Operations continue processing even if some fail
- **No cross-operation dependencies**: Failure of one operation doesn't affect others
- **Database consistency**: Each member update maintains data integrity independently

##### Security Features
- **Input validation**: All data validated before processing
- **Rate limiting**: Protects against abuse and system overload
- **Audit logging**: All operations logged with full traceability

##### Error Recovery
- **Independent operation failures**: Individual operations can fail without affecting others
- **No rollback between operations**: Each operation's success or failure is independent
- **Idempotency**: Safe to retry identical requests
- **Duplicate detection**: Automatic detection of duplicate member IDs

#### Request/Response Examples

**Important Note**: The examples below use fictional member data to demonstrate the global scope and diversity of operations possible across the Mensa International network. **In actual usage, each API request would be made by a single national Mensa organization using their own API key**, typically processing operations involving their own members or transfers specifically to/from their organization.

**Note on Email Domains**: The official Mensa domain examples are included solely for illustrative purposes. **In reality, members would never use their national Mensa organization's official domain as personal email addresses** - these domains are reserved exclusively for organizational use. Real members would use personal email providers (gmail.com, outlook.com, etc.) or their own domains.

##### 1. Join Action - Add New Members

Add new members to the national Mensa association.

###### New Member Registration

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "40011",
        "firstName": "Anna",
        "lastName": "Schmidt",
        "email": "anna.schmidt@mensa.de",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["40011"]
    }
  ]
}
```

**Response Headers**:

```http
X-Request-ID: req_abc123def456
Content-Type: application/json
```

###### Transfer from Another National Mensa

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "41022",
        "firstName": "Yuki",
        "lastName": "Tanaka",
        "email": "yuki.tanaka@mensa.jp",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "transferFromNationalMensaId": "NZ",
        "transferFromNationalMemberId": "123456"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["41022"]
    }
  ]
}
```

**Transfer Processing**:
- **Transfer coordination**: Operations coordinate with the source national Mensa
- **Member validation**: The `transferFromNationalMemberId` is validated against the source system
- **History preservation**: Member history is preserved during transfers
- **Direct MI members**: Use `nationalMensaId: "XX"` for direct Mensa International members in countries without a national Mensa organization
- **Transfer identification**: Transfers require both `transferFromNationalMensaId` and `transferFromNationalMemberId` fields

**Membership Types**:
- **Flexible conversion**: The destination national Mensa determines the membership type (lifetime or with expiration date)
- **Cross-type transfers**: A lifetime member from the source can receive an expiration date in the destination (and vice versa)

###### Transfer from Direct Mensa International Membership

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "55555",
        "firstName": "Maria",
        "lastName": "Silva",
        "email": "maria.silva@mensa.org.br",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "transferFromNationalMensaId": "XX",
        "transferFromNationalMemberId": "789456"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["55555"]
    }
  ]
}
```

**Direct MI Transfers**:
- Use `"XX"` as `transferFromNationalMensaId` for direct Mensa International transfers
- Applies to members from countries without a national Mensa organization
- The `transferFromNationalMemberId` will be the direct MI member ID
- Both transfer fields are required for transfer operations

###### Multiple New Members

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "42033",
        "firstName": "Eva",
        "lastName": "Novakova",
        "email": "eva.novakova@mensa.sk",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      },
      {
        "nationalMemberId": "43044",
        "firstName": "Thabo",
        "lastName": "Mthembu",
        "email": "thabo.mthembu@mensa.org.za",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-06-30T23:59:59.000Z",
        "transferFromNationalMensaId": "RS",
        "transferFromNationalMemberId": "789012"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["42033", "43044"]
    }
  ]
}
```

###### Returning Member (Former Member Rejoining)

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "33333",
        "firstName": "Sophie",
        "lastName": "Dubois",
        "email": "sophie.dubois@mensa-france.net",
        "membershipStartDate": "2025-01-15T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["33333"]
    }
  ]
}
```

**Returning Members**:
- Use the same `join` action for former members rejoining after >12 months
- `membershipStartDate` can be set to any desired start date for the new membership period
- Member history may be preserved from previous membership periods

###### Lifetime Membership

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "44444",
        "firstName": "Robert",
        "lastName": "Johnson",
        "email": "robert.johnson@us.mensa.org",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "lifetimeMembership": true
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["44444"]
    }
  ]
}
```

**Lifetime Membership**:
- Use `lifetimeMembership: true` for lifetime memberships
- Do not include `membershipEndDate` when `lifetimeMembership` is true
- Lifetime members can be converted to annual membership through the renew action

##### 2. Renew Action - Extend Memberships

Extend membership periods for existing active members. Each member renewal is processed individually.

###### Single Renewal Period

**Request**:

```json
[
  {
    "action": "renew",
    "data": [
      {
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "members": ["54321", "98765"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "renew",
      "success": true,
      "result": ["54321", "98765"]
    }
  ]
}
```

###### Multiple Renewal Periods

**Request**:

```json
[
  {
    "action": "renew",
    "data": [
      {
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "members": ["12901", "13402", "14503"]
      },
      {
        "membershipEndDate": "2025-06-30T23:59:59.000Z",
        "members": ["15604", "16705"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "renew",
      "success": true,
      "result": ["12901", "13402", "14503", "15604", "16705"]
    }
  ]
}
```

**Business Rules for Renewals**:
- Active members and recently expired members (within 12 months) can be renewed
- Lifetime members can be converted to annual membership by providing `membershipEndDate` (and omitting `lifetimeMembership`)
- Annual members can be converted to lifetime membership by providing `lifetimeMembership: true` (and omitting `membershipEndDate`)
- New `membershipEndDate` must be in the future
- Cannot renew excluded members
- For active members: renewal extends current membership period or converts membership type
- For expired members: renewal creates a new membership period starting from the renewal date
- Each member renewal is processed independently - some may succeed while others fail

###### Converting Annual Members to Lifetime Membership

**Request**:

```json
[
  {
    "action": "renew",
    "data": [
      {
        "lifetimeMembership": true,
        "members": ["17806", "18907"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "renew",
      "success": true,
      "result": ["17806", "18907"]
    }
  ]
}
```

###### Converting Lifetime Members to Annual Membership

**Request**:

```json
[
  {
    "action": "renew",
    "data": [
      {
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "members": ["45066"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "renew",
      "success": true,
      "result": ["45066"]
    }
  ]
}
```

###### Mixed Renewal Types

**Request**:

```json
[
  {
    "action": "renew",
    "data": [
      {
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "members": ["57188", "58199"]
      },
      {
        "lifetimeMembership": true,
        "members": ["59200", "60211"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "renew",
      "success": true,
      "result": ["57188", "58199", "59200", "60211"]
    }
  ]
}
```

##### 3. Leave Action - Member Departures

Process member departures from the national Mensa. Departures are categorized by reason with individual member processing.

###### Non-Renewal (No Transfer)

**Request**:

```json
[
  {
    "action": "leave",
    "data": [
      {
        "members": ["61222", "72333"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "leave",
      "success": true,
      "result": [
        {
          "members": ["61222", "72333"]
        }
      ]
    }
  ]
}
```

###### Transfer to Another National Mensa

**Request**:

```json
[
  {
    "action": "leave",
    "data": [
      {
        "members": ["44055"],
        "transferToNationalMensaId": "SG"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "leave",
      "success": true,
      "result": [
        {
          "transferToNationalMensaId": "SG",
          "members": ["44055"]
        }
      ]
    }
  ]
}
```

###### Mixed Leave Reasons

**Request**:

```json
[
  {
    "action": "leave",
    "data": [
      {
        "members": ["63244", "64255"]
      },
      {
        "members": ["65266", "66277"],
        "transferToNationalMensaId": "ID"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "leave",
      "success": true,
      "result": [
        {
          "members": ["73344", "74355"]
        },
        {
          "transferToNationalMensaId": "ID",
          "members": ["75366", "76377"]
        }
      ]
    }
  ]
}
```

##### 4. Exclude Action - Member Exclusions

Exclude members from the national Mensa (disciplinary action).

###### Single Exclusion

**Request**:

```json
[
  {
    "action": "exclude",
    "data": [
      {
        "members": ["25614"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "exclude",
      "success": true,
      "result": ["25614"]
    }
  ]
}
```

###### Multiple Exclusions

**Request**:

```json
[
  {
    "action": "exclude",
    "data": [
      {
        "members": ["26715", "27816", "28917"]
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "exclude",
      "success": true,
      "result": ["26715", "27816", "28917"]
    }
  ]
}
```

##### 5. Update Action - Member Information Updates

Update member information (name, email, etc.).

###### Single Field Update

**Request**:

```json
[
  {
    "action": "update",
    "data": [
      {
        "nationalMemberId": "53144",
        "email": "carlos.rodriguez.new@mensa.org.mx"
      }
    ]
  }
]
```

###### Multiple Fields Update

**Request**:

```json
[
  {
    "action": "update",
    "data": [
      {
        "nationalMemberId": "54155",
        "firstName": "Marco",
        "lastName": "Rossi",
        "email": "marco.rossi@mensa.it"
      }
    ]
  }
]
```

###### Multiple Members Update

**Request**:

```json
[
  {
    "action": "update",
    "data": [
      {
        "nationalMemberId": "55166",
        "email": "pablo.garcia.updated@mensa.es"
      },
      {
        "nationalMemberId": "56177",
        "firstName": "Li",
        "lastName": "Wei",
        "email": "li.wei.updated@mensa.org.sg"
      }
    ]
  }
]
```
**Response**:

```json
{
  "results": [
    {
      "action": "update",
      "success": true,
      "result": ["55166", "56177"]
    }
  ]
}
```

##### 6. Mixed Operations - Multiple Actions in Single Request

Combine multiple operation types in a single batch request.

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "99999",
        "firstName": "Emma",
        "lastName": "Wilson",
        "email": "emma.wilson@mensa.ca",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      },
      {
        "nationalMemberId": "88888",
        "firstName": "Ahmad",
        "lastName": "Hassan",
        "email": "ahmad.hassan@mensa.my",
        "membershipStartDate": "2025-02-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      }
    ]
  },
  {
    "action": "renew",
    "data": [
      {
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "members": ["67288", "68299"]
      }
    ]
  },
  {
    "action": "update",
    "data": [
      {
        "nationalMemberId": "69300",
        "email": "kim.minjun.updated@mensakorea.org"
      }
    ]
  },
  {
    "action": "leave",
    "data": [
      {
        "members": ["70311"],
        "transferToNationalMensaId": "PL"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["99999", "88888"]
    },
    {
      "action": "renew",
      "success": true,
      "result": ["67288", "68299"]
    },
    {
      "action": "update",
      "success": true,
      "result": ["69300"]
    },
    {
      "action": "leave",
      "success": true,
      "result": [
        {
          "transferToNationalMensaId": "PL",
          "members": ["70311"]
        }
      ]
    }
  ]
}
```

### Member Verification

#### POST /v1/members/verify

Verify membership for a traveling member from another national Mensa. This endpoint has strict rate limiting due to its sensitive nature.

**Request**:

```http
POST /v1/members/verify HTTP/1.1
Host: api.mensa.org
Content-Type: application/json
X-API-Key: your-api-key-here

{
  "nationalMensaId": "AU",
  "nationalMemberId": "46077",
  "firstName": "Sarah",
  "lastName": "Thompson"
}
```

**Response (Member Found)**:

```json
{
  "verified": true,
  "membershipStatus": "active",
  "membershipEndDate": "2025-12-31T23:59:59.000Z"
}
```

**Response (Lifetime Member Found)**:

```json
{
  "verified": true,
  "membershipStatus": "active",
  "lifetimeMember": true
}
```

**Response (Member Not Found or Inactive)**:

```json
{
  "verified": false
}
```

**Rate Limiting**: This endpoint has suggested strict rate limiting of **30 requests per hour per API key** to prevent abuse.

## Error Handling

### Batch Operations Error Handling

#### Individual Operation Errors

The batch endpoint uses a **partial success model** - individual operations can fail while others succeed. The response always includes detailed error information for failed operations.

**Response Design Philosophy**:
- **No root-level `success` field**: Eliminates ambiguity about "partial success" scenarios
- **Explicit operation results**: Clients must check each `results[].success` individually  
- **Clear error attribution**: Failed operations include detailed error information
- **Request tracing**: Use `X-Request-ID` for debugging and support requests

**Client Response Handling**:
Check each operation result individually - some may succeed while others fail within the same request.

**Request with Mixed Success/Failure**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "47088",
        "firstName": "Olaf",
        "lastName": "Hansen",
        "email": "olaf.hansen@mensa.no",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      },
      {
        "nationalMemberId": "48099",
        "firstName": "Wei",
        "lastName": "Chen",
        "email": "wei.chen@mensa.tw",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      }
    ]
  }
]
```

**Response**:

```json
{
  "results": [
    {
      "action": "join",
      "success": true,
      "result": ["47088"]
    },
    {
      "action": "join",
      "success": false,
      "result": [
        {
          "nationalMemberId": "48099",
          "errorCode": "MEMBER_ALREADY_EXISTS",
          "errorMessage": "Member with national ID '48099' already exists in NO",
          "field": "nationalMemberId",
          "retryable": false
        }
      ]
    }
  ]
}
```

#### Common Error Codes for Batch Operations

| Error Code | Description | Retryable | Common Causes |
|------------|-------------|-----------|---------------|
| `MEMBER_ALREADY_EXISTS` | Duplicate national member ID | No | Duplicate join attempts, data sync issues |
| `MEMBER_NOT_FOUND` | Member doesn't exist for update/renew/leave | No | Invalid member ID, member already processed |
| `INVALID_MEMBERSHIP_DATE` | Future/past date validation failed | No | Date format errors, business rule violations |
| `MEMBERSHIP_TYPE_CONFLICT` | Both lifetimeMembership and membershipEndDate provided | No | Mutually exclusive fields provided together |
| `TRANSFER_VALIDATION_FAILED` | Source/destination validation failed | No | Invalid country codes, member not found at source |
| `MEMBER_EXCLUDED` | Cannot process excluded member | No | Attempting to renew/update excluded member |
| `DATABASE_CONSTRAINT_VIOLATION` | Referential integrity error | Yes | Temporary DB issues, concurrent modifications |
| `EXTERNAL_SYSTEM_ERROR` | Transfer coordination failed | Yes | Network issues with other national Mensa systems |

### General API Errors

#### Authentication Errors

#### Missing API Key

**Request**:

```http
POST /v1/members/batch HTTP/1.1
Host: api.mensa.org
Content-Type: application/json

[]
```

**Response**:

```json
{
  "error": "Unauthorized",
  "message": "Missing X-API-Key header"
}
```

**Status Code**: 401

#### Invalid API Key

**Request**:

```http
POST /v1/members/batch HTTP/1.1
Host: api.mensa.org
Content-Type: application/json
X-API-Key: invalid-key

[]
```

**Response**:

```json
{
  "error": "Unauthorized",
  "message": "Invalid API key"
}
```

**Status Code**: 401

#### Inactive National Mensa

**Response**:

```json
{
  "error": "Forbidden",
  "message": "National Mensa is not active"
}
```

**Status Code**: 403

#### Validation Errors

#### Empty Batch Array

**Request**:

```json
[]
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "code": "too_small",
      "minimum": 1,
      "type": "array",
      "inclusive": true,
      "message": "At least one action is required",
      "path": []
    }
  ]
}
```

**Status Code**: 400

#### Invalid Action Type

**Request**:

```json
[
  {
    "action": "invalid_action",
    "data": []
  }
]
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "received": "invalid_action",
      "code": "invalid_union_discriminator",
      "options": ["join", "renew", "leave", "exclude", "update"],
      "path": [0, "action"],
      "message": "Invalid discriminator value. Expected 'join' | 'renew' | 'leave' | 'exclude' | 'update'"
    }
  ]
}
```

**Status Code**: 400

#### Missing Required Fields

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "49100",
        "firstName": "",
        "email": "invalid-email",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z"
      }
    ]
  }
]
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "code": "too_small",
      "minimum": 1,
      "type": "string",
      "inclusive": true,
      "message": "First name is required",
      "path": [0, "data", 0, "firstName"]
    },
    {
      "validation": "email",
      "code": "invalid_string",
      "message": "Must be a valid email address",
      "path": [0, "data", 0, "email"]
    },
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "undefined",
      "message": "Last name is required",
      "path": [0, "data", 0, "lastName"]
    }
  ]
}
```

**Status Code**: 400

#### Membership Type Conflict

**Request**:

```json
[
  {
    "action": "join",
    "data": [
      {
        "nationalMemberId": "50111",
        "firstName": "Alexandru",
        "lastName": "Popescu",
        "email": "alexandru.popescu@mensaromania.ro",
        "membershipStartDate": "2025-01-01T00:00:00.000Z",
        "membershipEndDate": "2025-12-31T23:59:59.000Z",
        "lifetimeMembership": true
      }
    ]
  }
]
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "code": "custom",
      "message": "Cannot specify both membershipEndDate and lifetimeMembership - they are mutually exclusive",
      "path": [0, "data", 0]
    }
  ]
}
```

**Status Code**: 400

#### Server Errors

#### Internal Server Error

**Response**:

```json
{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred",
  "requestId": "req_abc123def456",
  "timestamp": "2025-01-01T12:00:00.000Z"
}
```

**Status Code**: 500

### Member Verification Error Handling

#### Membership Verification Errors

#### Invalid National Mensa ID

**Request**:

```json
{
  "nationalMensaId": "INVALID",
  "nationalMemberId": "51122",
  "firstName": "Li",
  "lastName": "Wang"
}
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "code": "invalid_string",
      "message": "National Mensa ID must be a valid ISO 3166-1 alpha-2 code or 'XX' for direct Mensa International members",
      "path": ["nationalMensaId"]
    }
  ]
}
```

**Status Code**: 400

#### Missing Required Fields

**Request**:

```json
{
  "nationalMensaId": "JP",
  "nationalMemberId": "52133"
}
```

**Response**:

```json
{
  "error": "Validation Error",
  "message": "Invalid request payload",
  "details": [
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "undefined",
      "message": "First name is required",
      "path": ["firstName"]
    },
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "undefined",
      "message": "Last name is required",
      "path": ["lastName"]
    }
  ]
}
```

**Status Code**: 400

## Monitoring and Observability

### Request Tracing

Every API request receives a unique request ID for tracking and debugging:

```http
X-Request-ID: req_7f9e8d2c5b4a1f6e3d8c9a2b5e4f7e1d
```

Include this ID when contacting support or reporting issues.

#### Correlation ID Support

Clients can provide their own correlation ID for end-to-end request tracking:

```http
X-Correlation-ID: batch_sync_2025_07_22_001
```

**Response**: Correlation IDs are returned in response headers

### Health Check Monitoring

Use `/v1/health` for automated monitoring:

```http
GET /v1/health HTTP/1.1
Host: api.mensa.org
X-API-Key: your-api-key-here
```

**Status Values**:
- **ok**: All systems operational
- **degraded**: Some non-critical issues
- **down**: Critical systems unavailable

### Support Information

When contacting Mensa International support, include:

1. **Request ID**: `X-Request-ID` from the response
2. **Correlation ID**: If one was provided
3. **Timestamp**: When the issue occurred
4. **National Mensa ID**: Organization identifier
5. **Error details**: Complete error response if applicable

## Implementation Details

*This section contains detailed technical specifications for development teams.*

## Status Codes

| Code | Description           | Usage                                                      |
| ---- | --------------------- | ---------------------------------------------------------- |
| 200  | OK                    | Successful batch operation (even with individual failures) |
| 400  | Bad Request           | Invalid request payload or validation errors               |
| 401  | Unauthorized          | Missing or invalid API key                                 |
| 403  | Forbidden             | Valid API key but national Mensa is inactive               |
| 404  | Not Found             | Endpoint not found                                         |
| 429  | Too Many Requests     | Rate limit exceeded                                        |
| 500  | Internal Server Error | Unexpected server error                                    |

## Data Validation Rules

### Field Formats

| Field                          | Type    | Format                                    | Example                      |
| ------------------------------ | ------- | ----------------------------------------- | ---------------------------- |
| `nationalMemberId`             | String  | Non-empty string                          | `"12345"`                    |
| `firstName`                    | String  | Non-empty string                          | `"John"`                     |
| `lastName`                     | String  | Non-empty string                          | `"Doe"`                      |
| `email`                        | String  | Valid email format                        | `"john.doe@mensa.org.uk"`     |
| `membershipStartDate`          | String  | ISO 8601 date                             | `"2025-01-15T00:00:00.000Z"` |
| `membershipEndDate`            | String  | ISO 8601 date (mutually exclusive with lifetimeMembership) | `"2025-12-31T23:59:59.000Z"` |
| `lifetimeMembership`           | Boolean | True for lifetime memberships (mutually exclusive with membershipEndDate) | `true`                       |
| `transferFromNationalMensaId`  | String  | ISO 3166-1 alpha-2 code or "XX" for direct MI members | `"BE"`, `"IN"`, `"XX"` |
| `transferFromNationalMemberId` | String  | Non-empty string                          | `"789012"`                   |
| `transferToNationalMensaId`    | String  | ISO 3166-1 alpha-2 code or "XX" for direct MI members | `"AR"`, `"LU"`, `"FI"`, `"XX"` |

### Business Rules

1. **Join Action**:
   - `nationalMemberId`, `firstName`, `lastName`, `email`, and `membershipStartDate` are required
   - Either `membershipEndDate` OR `lifetimeMembership: true` is required (mutually exclusive)
   - For transfers: both `transferFromNationalMensaId` and `transferFromNationalMemberId` are required
   - `membershipStartDate` must be in the future (or current)
   - If `membershipEndDate` is provided, it must be after `membershipStartDate`
   - National member ID must be unique within the national Mensa

2. **Renew Action**:
   - `membershipEndDate` OR `lifetimeMembership: true` is required (mutually exclusive)
   - If `membershipEndDate` is provided, it must be in the future
   - All member IDs must exist and be active, recently expired (within 12 months), or lifetime members
   - Cannot renew excluded members
   - For active members: renewal extends current membership period or converts to lifetime
   - For recently expired members (≤12 months): renewal creates a new membership period starting from the renewal date
   - For lifetime members: can be converted to annual membership by providing `membershipEndDate` (without `lifetimeMembership`)

3. **Leave Action**:
   - Members can leave by not renewing their membership or transferring to another national Mensa
   - `transferToNationalMensaId` indicates a transfer to another national Mensa
   - Operations without `transferToNationalMensaId` are processed as non-renewal
   - Cannot process leave for already inactive members

4. **Exclude Action**:
   - No additional validations beyond member existence
   - Can exclude active or inactive members

5. **Update Action**:
   - At least one field must be provided for update
   - Email must be valid format if provided
   - Cannot update excluded members

6. **Membership Verification Action**:
   - `nationalMensaId`, `nationalMemberId`, `firstName`, and `lastName` are required
   - `nationalMensaId` must be a valid ISO 3166-1 alpha-2 country code or "XX" for direct Mensa International members
   - Returns verification status, membership validity, and membership type (lifetime or expiring)
   - For lifetime members, `lifetimeMember: true` is returned instead of `membershipEndDate`
   - No personal information beyond verification status is returned

## Rate Limiting

Rate limiting is implemented primarily as a security measure to prevent abuse in case of compromised systems.

### Suggested Limits

#### Batch Operations (`/v1/members/batch`)
- **Suggested Standard Limit**: 1000 requests per hour per API key
- **Suggested Burst Protection**: Maximum 100 requests per minute (to handle peak processing periods)

#### Membership Verification (`/v1/members/verify`)
- **Suggested Strict Limit**: 30 requests per hour per API key
- **Suggested Burst Protection**: Maximum 3 requests per minute (to prevent brute force attacks)
- **Purpose**: Prevent abuse of sensitive member information lookups and brute force attempts
- **Use Case**: Verification of traveling members (expected to be rare)

### Response Headers

All API responses include rate limiting information:

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1735689600
X-RateLimit-Window: 3600
```

**Note**: For `/v1/members/verify`, the `X-RateLimit-Limit` will show `30` instead of `1000`.

### Rate Limit Exceeded

When limits are exceeded, the API returns:

**Status Code**: 429 Too Many Requests

**Standard Rate Limit Response**:

```json
{
  "error": "Rate Limit Exceeded",
  "message": "Too many requests.",
  "retryAfter": 3600,
  "resetTime": "2025-01-01T13:00:00.000Z"
}
```

**Verification Burst Protection Response**:

```json
{
  "error": "Rate Limit Exceeded",
  "message": "Too many verification attempts.",
  "retryAfter": 45,
  "resetTime": "2025-01-01T12:01:00.000Z"
}
```

### Security Considerations

- Rate limits prevent abuse from compromised systems
- Burst protection prevents brute force attacks on verification endpoint
- Contact Mensa International if legitimate usage requires higher limits

### User-Agent Security (Optional)

API keys can optionally be associated with specific User-Agent patterns for enhanced security:
- Detect potential API key compromise
- Monitor client versions and usage patterns
- Automated alerts for unexpected patterns

## Best Practices

### General API Usage

- **API Keys**: Store securely and rotate regularly
- **Date Format**: Use ISO 8601 format with timezone
- **Request Tracing**: Include `X-Request-ID` in support requests
- **User-Agent**: Use descriptive, versioned strings (e.g., "MensaCA-Integration/2.1.0")

### Member Operations (Batch Endpoint)

#### Request Design
- **Group related operations**: Combine similar actions in logical batches
- **Optimal batch size**: 50-200 operations for best performance/reliability balance
- **Avoid mixing unrelated operations**: Keep exclusions separate from routine updates

#### Error Handling
- **Check individual results**: Always verify each `results[].success` value
- **Implement retry logic**: Use exponential backoff for failed operations
- **Handle partial success**: Some operations may succeed while others fail

#### Performance Guidelines
- **Small batches** (1-50): Real-time integrations
- **Medium batches** (50-200): Regular sync operations  
- **Large batches** (200-500): Bulk imports and annual renewals
- **Maximum batch size**: 500 operations (hard limit)

#### Security
- **Validate email formats** before sending
- **Ensure unique `nationalMemberId`** values within batches
- **Validate ISO country codes** for transfers
- **Input sanitization**: Clean data before API calls

### Member Verification Endpoint

- **Use sparingly**: Only for legitimate traveling member verification
- **Rate limiting awareness**: Monitor 30 requests/hour limit
- **Cache results**: Avoid repeated verification of same member

### Rate Limiting Strategy

- **Monitor headers**: Check `X-RateLimit-Remaining` and `X-RateLimit-Reset`
- **Implement backoff**: Use exponential backoff when approaching limits
- **Plan batch timing**: Distribute large operations across time windows

### Production Deployment

- **Idempotency**: Design requests to be safely retryable
- **Monitoring**: Implement proper logging and alerting
- **Graceful degradation**: Handle API unavailability scenarios
- **Connection pooling**: Reuse HTTP connections for better performance

---

*Last updated: July 28, 2025*