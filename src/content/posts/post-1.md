---
title: "API Versioning in real life"
description: "meta description"
date: 2024-04-01T05:00:00Z
image: "/images/posts/01.jpg"
categories: ["Backend","Frontend","Security","API"]
authors: ["Durga Chikkala"]
tags: ["backend", "api"]
draft: false
---


# API Versioning in Golang

Hello Everyone! Today, I am back with one of the most important topics: API Versioning.

## What is API Versioning?

API versioning is a crucial aspect of API design and development, ensuring that changes in your API do not disrupt existing clients. This guide explores four common methods for API versioning in Golang:

- **URI Versioning**
- **Header Versioning**
- **Query Parameter Versioning**
- **Media Type Versioning**

---

## 1. URI Versioning

URI versioning involves embedding the version number directly in the URI path. This method is straightforward and easy for clients to understand.

### Example

- **Version 1**: `GET /api/v1/products`
- **Version 2**: `GET /api/v2/products`

### Golang Implementation

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

// Handler for version 1 of the API
func getV1Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v1", "data":"This is v1 data"}`)
}

// Handler for version 2 of the API
func getV2Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v2", "data":"This is v2 data"}`)
}

// Main handler to route to the correct version
func mainHandler(w http.ResponseWriter, r *http.Request) {
    switch {
    case r.URL.Path == "/api/v1/data":
        getV1Data(w, r)
    case r.URL.Path == "/api/v2/data":
        getV2Data(w, r)
    default:
        http.NotFound(w, r)
    }
}

func main() {
    http.HandleFunc("/", mainHandler)
    log.Println("Starting server on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### Test it with the following curls

```bash
# Request to version 1 of the API
curl http://localhost:8080/api/v1/data

# Request to version 2 of the API
curl http://localhost:8080/api/v2/data
```

---

## 2. Header Versioning

In header versioning, you specify the API version in the HTTP headers. This method keeps the URI clean and focuses on headers for version control.

### Example

- **Version 1**: `GET /api/products` with `Headers: API-Version: v1`
- **Version 2**: `GET /api/products` with `Headers: API-Version: v2`

### Golang Implementation

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

// Handler for version 1 of the API
func getV1Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v1", "data":"This is v1 data"}`)
}

// Handler for version 2 of the API
func getV2Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v2", "data":"This is v2 data"}`)
}

// Main handler to route to the correct version based on headers
func mainHandler(w http.ResponseWriter, r *http.Request) {
    apiVersion := r.Header.Get("API-Version")
    switch apiVersion {
    case "v1":
        getV1Data(w, r)
    case "v2":
        getV2Data(w, r)
    default:
        http.Error(w, "Unsupported API version", http.StatusBadRequest)
    }
}

func main() {
    http.HandleFunc("/api/data", mainHandler)
    log.Println("Starting server on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### Test it with the following curls

```bash
# Request to version 1 of the API
curl -H "API-Version: v1" http://localhost:8080/api/data

# Request to version 2 of the API
curl -H "API-Version: v2" http://localhost:8080/api/data

# Request with an unsupported version
curl -H "API-Version: v3" http://localhost:8080/api/data
```

---

## 3. Query Parameter Versioning

With query parameter versioning, the version number is included as a query parameter in the request URL.

### Example

- **Version 1**: `GET /api/products?version=v1`
- **Version 2**: `GET /api/products?version=v2`

### Golang Implementation

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

// Handler for version 1 of the API
func getV1Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v1", "data":"This is v1 data"}`)
}

// Handler for version 2 of the API
func getV2Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintln(w, `{"version":"v2", "data":"This is v2 data"}`)
}

// Main handler to route to the correct version based on query parameter
func mainHandler(w http.ResponseWriter, r *http.Request) {
    queryParams := r.URL.Query()
    apiVersion := queryParams.Get("version")

    switch apiVersion {
    case "v1":
        getV1Data(w, r)
    case "v2":
        getV2Data(w, r)
    default:
        http.Error(w, "Unsupported API version", http.StatusBadRequest)
    }
}

func main() {
    http.HandleFunc("/api/data", mainHandler)
    log.Println("Starting server on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### Test it with the following curls

```bash
# Request to version 1 of the API
curl http://localhost:8080/api/data?version=v1

# Request to version 2 of the API
curl http://localhost:8080/api/data?version=v2

# Request with an unsupported version
curl http://localhost:8080/api/data?version=v3
```

---

## 4. Media Type Versioning

Also known as content negotiation, media-type versioning uses the `Accept` header to specify the version.

### Example

- **Version 1**: `GET /api/products` with `Headers: Accept: application/vnd.example.v1+json`
- **Version 2**: `GET /api/products` with `Headers: Accept: application/vnd.example.v2+json`

### Golang Implementation

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "strings"
)

// Handler for version 1 of the API
func getV1Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/vnd.example.v1+json")
    fmt.Fprintln(w, `{"version":"v1", "data":"This is v1 data"}`)
}

// Handler for version 2 of the API
func getV2Data(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/vnd.example.v2+json")
    fmt.Fprintln(w, `{"version":"v2", "data":"This is v2 data"}`)
}

// Main handler to route to the correct version based on Accept header
func mainHandler(w http.ResponseWriter, r *http.Request) {
    acceptHeader := r.Header.Get("Accept")
    if strings.Contains(acceptHeader, "application/vnd.example.v1+json") {
        getV1Data(w, r)
    } else if strings.Contains(acceptHeader, "application/vnd.example.v2+json") {
        getV2Data(w, r)
    } else {
        http.Error(w, "Unsupported API version", http.StatusUnsupportedMediaType)
    }
}

func main() {
    http.HandleFunc("/api/data", mainHandler)
    log.Println("Starting server on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

### Test it with the following curls

```bash
# Request version 1 of the API
curl -H "Accept: application/vnd.example.v1+json" http://localhost:8080/api/data

# Request version 2 of the API
curl -H "Accept: application/vnd.example.v2+json" http://localhost:8080/api/data

# Request with an unsupported version
curl -H "Accept: application/vnd.example.v3+json" http://localhost:8080/api/data
```

---

That's it for today! I hope this guide has provided valuable insights into API versioning techniques. Stay tuned for more topics.

Durga Chikkala signing off!
