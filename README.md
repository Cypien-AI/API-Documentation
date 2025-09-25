# Cypien AI API Documentation

**Version:** 2.0  
**Last Updated:** September 2025  
**Contact:** [developer@cypien.ai](mailto:developer@cypien.ai)

---

## İçerik Listesi

1. [API Overview](#api-overview)
2. [Authentication](#authentication)
3. [API Keys Management](#api-keys-management)
4. [Inventory Management](#inventory-management)
5. [Content Generation](#content-generation)
6. [Job Control](#job-control)
7. [Error Handling](#error-handling)
8. [Rate Limiting](#rate-limiting)

---

## API Overview

Cypien AI API, ürün görsellerinizi analiz ederek akıllı tag'ler oluşturur ve marka kimliğinize uygun içerikler üretir.

### Two-Step Process

1. **Image-to-Tag Generation:** Ürün resimlerinden AI ile tag üretimi
2. **Content Generation:** Onaylanan tag'lerle içerik oluşturma

### Base URLs

- **Production:** `https://api.cypien.ai/`

### Supported Content Types

- Product Title
- Product Description
- Google Feed
- Image to TAG

---

## Authentication

### Bearer Token Authentication

- **Base URL:** `https://api.cypien.com/v1`
- **Authentication:** Bearer Token (JWT)
- **Content-Type:** `application/json`

#### Login

```http
POST /v1/auth/login
Content-Type: application/json

{
    "apiKey": "your_api_key",
    "workspaceId": 123
}
```

**Response:**

```json
{
  "access_token": "eyJ...",
  "token_type": "Bearer",
  "expires_in": 86400,
  "workspace": {
    "id": 123,
    "name": "My Workspace"
  },
  "permissions": [
    "generate_tags",
    "generate_content",
    "generate_title",
    "generate_google_feed",
    "view_analytics",
    "manage_brands"
  ],
  "rateLimit": "professional"
}
```

#### Get User Info

```http
GET /v1/auth/me
Authorization: Bearer {token}
```

---

## API Keys Management

### List API Keys

```http
GET /v1/api-keys
Authorization: Bearer {token}
```

### Create API Key

```http
POST /v1/api-keys
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "Production API Key",
    "permissions": ["generate_content", "view_analytics"],
    "rateLimit": "professional",
    "workspaceId": 123
}
```

### Delete API Key

```http
DELETE /v1/api-keys/{keyId}
Authorization: Bearer {token}
```

---

## Inventory Management

### Get Products

```http
GET /v1/inventory/products
Authorization: Bearer {token}
```

**Response:**

```json
{
  "products": [
    {
      "id": "prod_123",
      "sku_id": "SKU-001",
      "title": "Premium Leather Bag",
      "description": "<p>High-quality leather handbag</p>",
      "categories": ["Fashion", "Bags"],
      "pictures": ["image1.jpg", "image2.jpg"],
      "primary_picture": "image1.jpg",
      "tags": ["leather", "premium", "handbag"],
      "suggested_tags": ["luxury", "fashion"],
      "workspace_id": 123,
      "created_at": "2025-09-22T10:00:00Z",
      "updated_at": "2025-09-22T10:00:00Z",
      "deleted": false,
      "has_suggested_tags": true,
      "ready_for_tagging": true
    }
  ],
  "total_count": 57,
  "_metadata": {
    "processing_time_ms": 1311,
    "workspace_id": 123,
    "total_products": 57,
    "data_optimized": true
  }
}
```

### Get Product Variants

```http
GET /v1/inventory/variants/{inventoryProductId}
Authorization: Bearer {token}
```

**Response:**

```json
{
  "success": true,
  "message": "Retrieved all variants for product Unisex T-shirt",
  "inventoryProductId": 13302,
  "productName": "Unisex T-shirt",
  "tags": [
    "günlük stil",
    "neşeli tasarım",
    "rahat kesim",
    "portakal desenli",
    "meyve motifli",
    "krem rengi",
    "güneşli günler",
    "pozitif enerji",
    "doğal esintiler",
    "yazlık tişört"
  ],
  "descriptionVariants": [
    {
      "uid": "e64bbcd6-3560-497c-8bff-7090c1d24dc7",
      "productDescriptionId": 10890,
      "workspacePersonaId": 66,
      "specialDayId": 0,
      "baseAudienceId": 1761,
      "body": "",
      "language": "en-US",
      "status": "WAITING",
      "createdAt": "2025-09-22T15:26:09.602Z",
      "updatedAt": "2025-09-22T15:26:09.602Z",
      "personaName": "SIMPLE_VISITOR",
      "specialDayName": "",
      "audienceName": "test"
    }
  ],
  "titleVariants": [
    {
      "uid": "c34410ba-4ca5-4e58-be22-28765c9602b0",
      "productTitleId": 10890,
      "workspacePersonaId": 66,
      "specialDayId": 0,
      "baseAudienceId": 1761,
      "body": "",
      "language": "en-US",
      "status": "WAITING",
      "createdAt": "2025-09-11T11:32:02.161Z",
      "updatedAt": "2025-09-11T11:32:02.161Z",
      "personaName": "SIMPLE_VISITOR",
      "specialDayName": "",
      "audienceName": "test"
    }
  ],
  "googleFeedVariants": [
    {
      "uid": "978ed7bc-cccf-4d91-9479-fbb30cba6d62",
      "productGoogleFeedId": 10840,
      "workspacePersonaId": 66,
      "specialDayId": 0,
      "baseAudienceId": 1761,
      "body": "",
      "language": "en-US",
      "status": "WAITING",
      "createdAt": "2025-09-11T15:20:02.207Z",
      "updatedAt": "2025-09-11T15:20:02.207Z",
      "personaName": "SIMPLE_VISITOR",
      "specialDayName": "",
      "audienceName": ""
    }
  ],
  "retrievedAt": "2025-09-22T18:35:05.329Z"
}
```

---

## Content Generation

### AI-Powered Content Generation Endpoints

#### Generate Content

```http
POST /v1/content/generate
Authorization: Bearer {token}
Content-Type: application/json

{
    "workspace_id": "123",
    "content_types": ["description"],
    "language": "tr-TR"
}
```

**Response:**

```json
{
  "job_id": "content_123_1695384000_abc123",
  "processing_mode": "async",
  "status": "queued",
  "estimated_completion": "2025-09-22T16:02:41.189Z",
  "status_url": "/jobs/content_123_1695384000_abc123/status",
  "products_found": 17,
  "products_queued": 17,
  "rate_limit_info": {
    "hourly_remaining": 85,
    "daily_remaining": 450,
    "next_reset_at": "2025-09-22T17:00:00.000Z"
  },
  "rate_limit_estimate": {
    "total_variants_to_generate": 51,
    "rate_limit_requests_needed": 51,
    "segments_count": 3,
    "interests_count": 1
  },
  "processing_config": {
    "mode": "background",
    "estimated_batches": 3
  }
}
```

#### Generate Title

```http
POST /v1/content/generate-title
Authorization: Bearer {token}
Content-Type: application/json

{
    "workspace_id": "prod_123",
    "language": "tr-TR",
    "title_types": ["title"]
}
```

**Response:**

```json
{
  "job_id": "title_123_1695384000_def456",
  "processing_mode": "async",
  "status": "queued",
  "estimated_completion": "2025-09-22T16:05:41.189Z",
  "status_url": "/jobs/title_123_1695384000_def456/status",
  "products_found": 1,
  "products_queued": 1,
  "rate_limit_info": {
    "hourly_remaining": 84,
    "daily_remaining": 449,
    "next_reset_at": "2025-09-22T17:00:00.000Z"
  },
  "rate_limit_estimate": {
    "total_variants_to_generate": 12,
    "rate_limit_requests_needed": 12,
    "segments_count": 3,
    "interests_count": 1
  }
}
```

#### Generate Google Feed Content

```http
POST /v1/content/generate-google-feed
Authorization: Bearer {token}
Content-Type: application/json

{
    "workspace_id": "prod_123",
    "language": "tr-TR",
    "googlefeed_types": ["google_feed"]
}
```

**Response:**

```json
{
  "job_id": "google_feed_123_1695384000_ghi789",
  "processing_mode": "async",
  "status": "queued",
  "estimated_completion": "2025-09-22T16:08:41.189Z",
  "status_url": "/jobs/google_feed_123_1695384000_ghi789/status",
  "products_found": 2,
  "products_queued": 2,
  "rate_limit_info": {
    "hourly_remaining": 82,
    "daily_remaining": 447,
    "next_reset_at": "2025-09-22T17:00:00.000Z"
  },
  "rate_limit_estimate": {
    "total_variants_to_generate": 24,
    "rate_limit_requests_needed": 24,
    "segments_count": 3,
    "interests_count": 1
  }
}
```

#### Generate Tags (Auto)

```http
POST /v1/tags/generate
Authorization: Bearer {token}
Content-Type: application/json

{
    "workspace_id": "69",
    "mode": "auto"
}
```

**Response:**

```json
{
  "job_id": "no_products_1758297482555",
  "processing_mode": "sync",
  "products_found": 0,
  "products_queued": 0,
  "products_without_images": 0,
  "results": [],
  "rate_limit_info": {
    "hourly_remaining": 40,
    "daily_remaining": 499,
    "next_reset_at": "2025-09-19T16:00:00.000Z"
  },
  "processing_config": {
    "mode": "none",
    "batch_size": 0,
    "estimated_batches": 0
  },
  "messages": [
    {
      "type": "warning",
      "message": "No products found for tagging"
    }
  ]
}
```

---

## Job Control

### Job Status

```http
GET /v1/jobs/{jobId}/status
Authorization: Bearer {token}
```

**Response:**

```json
{
  "job_id": "content_69_1758277165018_jq0u5i",
  "job_type": "content",
  "status": "completed",
  "processing_mode": "async",
  "workspace_id": 69,
  "brand_id": "69",
  "progress": {
    "total_products": 18,
    "processed_products": 18,
    "success_products": 18,
    "failed_products": 0,
    "percentage": 100
  },
  "timing": {
    "created_at": "2025-09-19T10:19:25.018Z",
    "started_at": "2025-09-19T10:20:00.694Z",
    "completed_at": "2025-09-19T10:24:06.396Z"
  },
  "configuration": {
    "processing_mode": "async",
    "batch_size": 10,
    "retry_limit": 3,
    "webhook_configured": false
  }
}
```

### Control Job Execution

```http
PUT /v1/jobs/{jobId}/control
Authorization: Bearer {token}
Content-Type: application/json

{
    "action": "pause" // pause, resume, cancel
}
```

**Parameters:**

- `action` (string): İşlem türü
  - `pause`: Job'u duraklat
  - `resume`: Duraklatılmış job'u devam ettir
  - `cancel`: Job'u iptal et

**Response:**

```json
{
  "success": true,
  "job_id": "content_123_1695384000_abc123",
  "action": "pause",
  "previous_status": "processing",
  "current_status": "paused",
  "message": "Job paused successfully"
}
```

---

## Error Handling

API tüm endpoint'lerde standart HTTP status kodları kullanır:

- **400 BAD_REQUEST** - Hatalı İstek: İstemci tarafından gönderilen istek geçersiz veya hatalı
- **401 UNAUTHORIZED** - Yetkisiz: Kimlik doğrulaması gerekli veya başarısız
- **403 FORBIDDEN** - Yasak: Kimlik doğrulanmış ama yetki yok
- **404 NOT_FOUND** - Bulunamadı: İstenen kaynak mevcut değil
- **429 TOO_MANY_REQUESTS** - Çok Fazla İstek: Rate limit aşıldı
- **500 INTERNAL_SERVER_ERROR** - Sunucu Hatası: Beklenmeyen sunucu hatası

### Rate Limiting Error Codes

- **HOURLY_RATE_LIMIT_EXCEEDED** - Saatlik rate limit aşıldı
- **DAILY_RATE_LIMIT_EXCEEDED** - Günlük rate limit aşıldı
- **CONCURRENT_LIMIT_EXCEEDED** - Eşzamanlı işlem limiti aşıldı

### Error Response Format

```json
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "API key not found or invalid",
    "details": "API key not found or invalid",
    "timestamp": "2025-09-23T14:14:26.532Z",
    "path": "/v1/auth/login"
  }
}
```

#### Rate Limit Error Examples

```json
// Saatlik limit aşımı
{
    "error": {
        "code": "HOURLY_RATE_LIMIT_EXCEEDED",
        "message": "Hourly rate limit exceeded",
        "details": "You have exceeded your hourly rate limit of 200 requests. Please wait until the next hour or upgrade your plan.",
        "timestamp": "2025-09-23T14:14:26.532Z",
        "path": "/v1/content/generate"
    }
}

// Günlük limit aşımı
{
    "error": {
        "code": "DAILY_RATE_LIMIT_EXCEEDED",
        "message": "Daily rate limit exceeded",
        "details": "You have exceeded your daily rate limit of 1600 requests. Please wait until tomorrow or upgrade your plan.",
        "timestamp": "2025-09-23T14:14:26.532Z",
        "path": "/v1/content/generate"
    }
}

// Eşzamanlı işlem limiti
{
    "error": {
        "code": "CONCURRENT_LIMIT_EXCEEDED",
        "message": "Concurrent job limit exceeded",
        "details": "You have reached the maximum number of concurrent jobs (5). Please wait for existing jobs to complete.",
        "timestamp": "2025-09-23T14:14:26.532Z",
        "path": "/v1/content/generate"
    }
}
```

---

## Rate Limiting

### 🚦 Plan-Based Rate Limiting System

| Plan         | Saatlik Limit | Günlük Limit   | Eşzamanlı Job Limit | Özellikler          |
| ------------ | ------------- | -------------- | ------------------- | ------------------- |
| Standard     | 50 requests   | 800 requests   | 1 concurrent job    | Temel özellikler    |
| Professional | 100 requests  | 1,600 requests | 3 concurrent jobs   | Gelişmiş özellikler |
| Enterprise   | 200 requests  | 3,200 requests | 10 concurrent jobs  | Tam özellik seti    |

### Environment Configuration

```env
# Standard Plan
RATE_LIMIT_STANDARD_HOURLY=50
RATE_LIMIT_STANDARD_DAILY=800
RATE_LIMIT_STANDARD_CONCURRENT=1

# Professional Plan
RATE_LIMIT_PROF_HOURLY=100
RATE_LIMIT_PROF_DAILY=1600
RATE_LIMIT_PROF_CONCURRENT=3

# Enterprise Plan
RATE_LIMIT_ENT_HOURLY=200
RATE_LIMIT_ENT_DAILY=3200
RATE_LIMIT_ENT_CONCURRENT=10
```

### Rate Limiting Davranışı

- **Saatlik Limit Aşımı:** İşlem bir sonraki saate devreder
- **Günlük Limit Kontrolü:** Günlük limit dahilinde tamamlanır
- **Günlük Limit Aşımı:** İşlem bir sonraki güne devreder
- **Otomatik Devretme:** Rate limit aşımında işlemler otomatik olarak sonraki uygun zaman dilimine planlanır

---

## Support & Contact

- **Technical Support:** [developer@cypien.ai](mailto:developer@cypien.ai)

---

© 2025 Cypien AI - Version 2.0
