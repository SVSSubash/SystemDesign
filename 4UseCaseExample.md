# UC-002: Validate URL Format

## Use Case Description
Validate that the user-provided URL is properly formatted and accessible.

## Goals
**Business Goals** (Why do you want to achieve this? - Business value motivation)
  - *Example*: Increase revenue by 20% through improved online sales conversion
  - *Example*: Reduce patient wait times by 30% through efficient appointment scheduling
  - *Example*: Improve customer satisfaction by providing 24/7 self-service banking capabilities
  - Ensure all URLs processed by the system are properly formatted according to RFC 3986 standards
  - Verify URL accessibility and reachability before further processing
  - Maintain system security by screening URLs for malicious content
  - Provide clear feedback to users when URL validation fails
  - Support both individual and batch URL validation workflows
  
**Functionality Goals** (Why do you want to achieve this? - Functionality perspective)
  - *Example*: Enable seamless product discovery and purchase workflow
  - *Example*: Provide real-time appointment availability and booking confirmation
  - *Example*: Ensure secure, instant fund transfers with transaction verification

## Boundaries
**In Scope:**
- URL format validation (protocol, domain, path structure)
- Basic accessibility checking (HTTP response validation)
- Security screening for malicious content
- User feedback for validation failures
- Admin batch processing capabilities

**Out of Scope:**
- Content analysis of the target webpage
- Deep security scanning of linked resources
- URL shortening or transformation
- Permanent storage of validation results
- Integration with external URL databases beyond security screening

## Actors
- System
- External URL endpoint

## Preconditions
- User has entered URL in text box
- UC-001 completed successfully

## Main Flow
- **Importance Category**
  - Primary: Core business-critical steps
  - Secondary: Supporting or enhancement steps

- **Abstraction Category**  
  - High level: Business process steps
  - Low level: Technical implementation steps

- **Steps**
1. System receives URL from user input
   - **Success**: URL captured successfully → Continue to step 2
   - **Failure**: Input validation error → Display "Invalid input format"
   - **Expected Result**: Clean URL string stored in system memory, ready for format validation
   - **Not Expected Result**: Malformed input, empty string, or system unable to capture user input
   - **NFR Implementation**:
     - **Performance**: Input processing <10ms to maintain overall 5s target
     - **Security**: Input sanitization prevents injection attacks
     - **Scalability**: Stateless processing enables horizontal scaling
     - **Reliability**: Input validation reduces downstream failures by 95%
   - **Architecture Components**:
     - **Load Balancer**: Nginx/AWS ALB distributes incoming requests
     - **API Gateway**: Kong/AWS Gateway handles request routing and validation
     - **Input Validator**: Custom service for schema validation and sanitization
     - **Rate Limiter**: Redis-based throttling to prevent abuse
     - **Monitoring**: Prometheus metrics collection for request rates

2. System validates URL format (regex check)
   - **Success**: URL matches valid pattern → Continue to step 3
   - **Failure**: Regex validation fails → Display "Please enter a valid URL" → User corrects input
   - **Expected Result**: URL conforms to RFC 3986 standard with proper protocol, domain, and path structure
   - **Not Expected Result**: URL missing protocol, invalid characters, or malformed domain structure
   - **NFR Implementation**:
     - **Performance**: Regex validation completes in <50ms for 99% of URLs
     - **Maintainability**: Configurable regex patterns for easy updates
     - **Reliability**: 99.9% accuracy rate in format detection
     - **Usability**: Clear error messages guide user correction
   - **Architecture Components**:
     - **Validation Service**: Node.js/Spring Boot microservice
     - **Regex Engine**: Built-in pattern matching with RFC 3986 compliance
     - **Pattern Cache**: Redis cache for compiled regex patterns
     - **Configuration Store**: AWS Parameter Store for pattern management
     - **Audit Logger**: Structured logging for validation patterns

3. System checks URL accessibility (HTTP request)
   - **Success**: URL responds within 5 seconds → Continue to step 4
   - **Failure**: Request times out or fails → Display "URL is not accessible" → User provides different URL
   - **Expected Result**: HTTP response received with status code 200-299, indicating reachable endpoint
   - **Not Expected Result**: Connection timeout, DNS resolution failure, or HTTP error codes (4xx, 5xx)
   - **NFR Implementation**:
     - **Performance**: 5-second timeout enforces response time requirement
     - **Scalability**: Connection pooling supports 1000 concurrent requests
     - **Reliability**: 3 retry attempts with exponential backoff
     - **Operability**: Circuit breaker prevents cascade failures
   - **Architecture Components**:
     - **HTTP Client Pool**: Axios/OkHttp with 100 concurrent connections
     - **Connection Manager**: Manages persistent connections and pooling
     - **Circuit Breaker**: Hystrix/Resilience4j with 50% failure threshold
     - **DNS Resolver**: Custom/system resolver with caching
     - **Retry Handler**: Exponential backoff with jitter

4. System confirms URL is reachable and safe
   - **Success**: URL is validated and safe to process → Continue to post-conditions
   - **Failure**: URL flagged by security service → Display "URL not allowed" → User provides different URL
   - **Expected Result**: URL passes security screening with no malicious content or blacklisted domains
   - **Not Expected Result**: URL flagged as malware, phishing, or contains prohibited content types
   - **NFR Implementation**:
     - **Security**: 100% threat screening coverage as required
     - **Compliance**: All security events logged for audit trail
     - **Performance**: Cached security results reduce scan time by 80%
     - **Reliability**: Fallback to cached blacklist if external service fails
   - **Architecture Components**:
     - **Security Service**: Dedicated microservice for threat detection
     - **Blacklist Database**: PostgreSQL with B-tree indexes for fast lookups
     - **External Scanner Integration**: VirusTotal/URLVoid API clients
     - **Threat Intelligence**: Real-time threat feed integration
     - **Security Cache**: Redis cache for scan results (TTL: 1 hour)
     - **Compliance Logger**: Immutable audit trail in Elasticsearch

## Post-conditions
- URL is validated and safe to process
- System ready for database check

## Related Use Cases
- Prerequisite: UC-001 (Service Availability)
- Triggers: UC-003 (Check URL Existence)

## Alternative Flow (Admin Batch Processing)

### Alternative Flow Preconditions
- Admin user has appropriate permissions for batch operations
- System has sufficient resources to handle batch processing
- Batch file is available and accessible
- UC-001 completed successfully

- **Importance Category**
  - Primary: Core business-critical steps
  - Secondary: Supporting or enhancement steps

- **Abstraction Category**  
  - High level: Business process steps
  - Low level: Technical implementation steps

- **Steps**
1. Admin uploads batch file containing multiple URLs
   - **Success**: File uploaded and parsed successfully → Continue to step 2
   - **Failure**: File upload error or unsupported format → Display "Please upload a valid CSV/TXT file"
   - **Expected Result**: Batch file containing URLs parsed into individual URL entries for processing
   - **Not Expected Result**: Corrupted file, unsupported format, or file size exceeds system limits
   - **NFR Implementation**:
     - **Performance**: File parsing completes within 30 seconds for 10,000 URLs
     - **Scalability**: Supports batch files up to 100MB
     - **Security**: File type validation prevents malicious uploads
     - **Usability**: Progress indicators for large batch uploads
   - **Architecture Components**:
     - **File Upload Service**: Multipart file handling with progress tracking
     - **File Parser**: CSV/TXT parser with validation
     - **Object Storage**: AWS S3/Azure Blob for temporary file storage
     - **Batch Queue**: Redis/RabbitMQ for processing queue management
     - **Progress Tracker**: WebSocket connections for real-time updates

2. System validates format for each URL in batch (regex check)
   - **Success**: All URLs match valid patterns → Continue to step 3
   - **Failure**: One or more URLs fail validation → Generate validation report with errors
   - **Expected Result**: All URLs conform to RFC 3986 standard with proper structure
   - **Not Expected Result**: URLs with missing protocols, invalid characters, or malformed structures
   - **NFR Implementation**:
     - **Performance**: Parallel validation of 100 URLs per second
     - **Scalability**: Horizontal scaling with worker nodes
     - **Reliability**: Error isolation prevents one bad URL from failing entire batch
     - **Observability**: Detailed validation reports with error categorization
   - **Architecture Components**:
     - **Batch Processor**: Parallel processing framework (Celery/Sidekiq)
     - **Worker Nodes**: Auto-scaling validation workers
     - **Result Aggregator**: Collects and consolidates validation results
     - **Report Generator**: PDF/CSV report generation service
     - **Error Handler**: Categorizes and tracks validation failures

3. System checks accessibility for all URLs in batch (parallel HTTP requests)
   - **Success**: All URLs respond within timeout limits → Continue to step 4
   - **Failure**: One or more URLs fail accessibility check → Log failures and continue processing
   - **Expected Result**: HTTP responses received for all URLs with status codes 200-299
   - **Not Expected Result**: Connection timeouts, DNS failures, or HTTP error codes for any URLs
   - **NFR Implementation**:
     - **Performance**: Concurrent processing with configurable parallelism
     - **Scalability**: Dynamic worker scaling based on queue depth
     - **Reliability**: Graceful handling of partial failures
     - **Resource Management**: Connection pooling prevents resource exhaustion
   - **Architecture Components**:
     - **Parallel HTTP Client**: Concurrent request processor
     - **Load Balancer**: Distributes outbound requests across multiple IPs
     - **Request Scheduler**: Manages request timing to prevent rate limiting
     - **Result Collector**: Aggregates accessibility check results
     - **Failure Handler**: Categorizes and reports accessibility failures

4. System confirms all URLs are reachable and safe
   - **Success**: All URLs pass security screening → Continue to post-conditions
   - **Failure**: One or more URLs flagged by security → Generate security report
   - **Expected Result**: All URLs pass security screening with no malicious content detected
   - **Not Expected Result**: URLs flagged as malware, phishing, or containing prohibited content
   - **NFR Implementation**:
     - **Security**: Comprehensive threat analysis for all URLs
     - **Performance**: Batch security scanning with result caching
     - **Compliance**: Detailed security audit trail for all scanned URLs
     - **Cost Optimization**: Cached results reduce external API costs
   - **Architecture Components**:
     - **Batch Security Scanner**: Bulk security analysis service
     - **External API Manager**: Rate-limited integration with security services
     - **Security Report Generator**: Comprehensive threat analysis reports
     - **Alert System**: Real-time notifications for detected threats
     - **Cost Monitor**: Tracks external API usage and costs

## Alternative Flow Post-conditions
- Batch URL validation completed with detailed report
- System ready for bulk database operations
- Admin receives comprehensive validation summary

## Exceptional flow (Exception flows are indeed alternate flows for exceptional circumstances, not error handling within normal flows.)

### Exceptional flow preconditions
- System maintenance window is scheduled or emergency maintenance required
- User attempts URL validation during maintenance period
- Maintenance mode is activated in the system

- **Importance Category**
  - Primary: Core business-critical steps
  - Secondary: Supporting or enhancement steps

- **Abstraction Category**  
  - High level: Business process steps
  - Low level: Technical implementation steps

- **Steps**
1. System detects maintenance mode is active
   - **Success**: Maintenance mode detected → Continue to step 2
   - **Failure**: Maintenance mode detection fails → Continue with normal flow (potential system instability)
   - **Expected result**: System gracefully handles maintenance state
   - **Not expected result**: System continues normal processing during maintenance

2. System displays maintenance message to user
   - **Success**: User receives clear maintenance notification → End flow
   - **Failure**: Message display fails → Log error and attempt fallback notification
   - **Expected result**: "System is under maintenance. Please try again in 30 minutes."
   - **Not expected result**: User receives generic error or no notification

### Exceptional flow postconditions
- User is informed of maintenance window
- No URL validation is attempted during maintenance
- System maintains data integrity during maintenance period