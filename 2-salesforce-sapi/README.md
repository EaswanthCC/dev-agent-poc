# Salesforce System API (SAPI)

A MuleSoft API that provides secure access to Salesforce Lead operations with OAuth 2.0 authentication, comprehensive error handling, and JSON-based request/response format.

## Features

- **CRUD Operations**: Complete Create, Read, Update, Delete operations for Salesforce Leads
- **OAuth 2.0 Security**: Secure authentication and authorization with Salesforce
- **Query Filtering**: Support for filtering leads by status and company with pagination
- **Error Handling**: Comprehensive error handling with meaningful error messages
- **Health Check**: Built-in health monitoring endpoint
- **APIKit Integration**: Full APIKit support with auto-generated console
- **Scalable Design**: Connection pooling and performance optimizations

## API Endpoints

### Lead Operations

- `GET /api/v1/leads` - Query leads with optional filters
  - Query Parameters:
    - `limit` (optional): Maximum number of records (default: 50, max: 200)
    - `offset` (optional): Number of records to skip (default: 0)
    - `status` (optional): Filter by lead status
    - `company` (optional): Filter by company name (partial match)

- `POST /api/v1/leads` - Create a new lead
  - Required fields: `LastName`, `Company`
  - Optional fields: `FirstName`, `Email`, `Phone`, `Status`, `LeadSource`, `Description`

- `GET /api/v1/leads/{leadId}` - Get a specific lead by ID

- `PUT /api/v1/leads/{leadId}` - Update an existing lead
  - Same payload structure as POST

- `DELETE /api/v1/leads/{leadId}` - Delete a lead by ID

### Health Check

- `GET /api/v1/health` - Health check endpoint
  - Returns API and Salesforce connectivity status

### API Console

- `GET /console` - APIKit console for testing endpoints

## Configuration

### Required Salesforce Setup

1. **Connected App Configuration**:
   - Create a Connected App in Salesforce
   - Enable OAuth settings
   - Note down Consumer Key and Consumer Secret

2. **User Credentials**:
   - Salesforce username and password
   - Security token (if required by your org settings)

### Configuration Properties

Update `src/main/resources/config.properties` with your Salesforce credentials:

```properties
# HTTP Configuration
http.port=8081

# Salesforce Configuration
salesforce.username=your-salesforce-username@domain.com
salesforce.password=your-salesforce-password
salesforce.security.token=your-salesforce-security-token
salesforce.consumer.key=your-connected-app-consumer-key
salesforce.consumer.secret=your-connected-app-consumer-secret

# Salesforce URLs (Change for production)
salesforce.url=https://login.salesforce.com/services/Soap/u/58.0
salesforce.authorization.url=https://login.salesforce.com/services/oauth2/authorize
salesforce.token.endpoint=https://login.salesforce.com/services/oauth2/token
salesforce.callback.url=http://localhost:8081/callback
```

## Project Structure

```
2-salesforce-sapi/
├── src/main/
│   ├── mule/
│   │   ├── 2-salesforce-sapi.xml      # Main flow definitions
│   │   └── global.xml                 # Global configurations
│   └── resources/
│       ├── api/
│       │   └── salesforce-sapi.raml   # API specification
│       ├── config.properties          # Configuration properties
│       └── log4j2.xml                # Logging configuration
├── pom.xml                           # Maven dependencies
└── README.md                         # This file
```

## Dependencies

- **Mule Runtime**: 4.11.0
- **Salesforce Connector**: 10.21.0
- **HTTP Connector**: 1.7.3
- **APIKit Module**: 1.10.4
- **Sockets Connector**: 1.2.3

## Error Handling

The API implements comprehensive error handling for:

- **Salesforce Connectivity Issues** (503 Service Unavailable)
- **Authentication Failures** (401 Unauthorized)
- **Invalid Requests** (400 Bad Request)
- **Resource Not Found** (404 Not Found)
- **General Errors** (500 Internal Server Error)

All errors return JSON responses with:
```json
{
  "message": "Error description",
  "code": 400,
  "details": "Additional error details"
}
```

## Scalability Features

1. **Connection Pooling**: Configured for optimal performance
2. **Timeout Management**: Proper timeout settings for HTTP and Salesforce operations
3. **Pagination Support**: Built-in pagination for large datasets
4. **Efficient SOQL Queries**: Optimized Salesforce queries with proper filtering
5. **Memory Management**: Streaming and efficient data transformations

## Running the Application

1. **Prerequisites**:
   - Java 17 or higher
   - Maven 3.6 or higher
   - Anypoint Studio (optional, for development)

2. **Build and Run**:
   ```bash
   cd 2-salesforce-sapi
   mvn clean package
   mvn mule:run
   ```

3. **Access the API**:
   - Base URL: `http://localhost:8081/api/v1`
   - Console: `http://localhost:8081/console`
   - Health Check: `http://localhost:8081/api/v1/health`

## Example Requests

### Create a Lead
```bash
curl -X POST http://localhost:8081/api/v1/leads \
  -H "Content-Type: application/json" \
  -d '{
    "FirstName": "John",
    "LastName": "Doe", 
    "Company": "ACME Corp",
    "Email": "john.doe@acme.com",
    "Phone": "+1234567890",
    "Status": "Open - Not Contacted",
    "LeadSource": "Web"
  }'
```

### Query Leads with Filters
```bash
curl "http://localhost:8081/api/v1/leads?status=Open - Not Contacted&limit=10&offset=0"
```

### Update a Lead
```bash
curl -X PUT http://localhost:8081/api/v1/leads/00QXX0000004C5OMAX \
  -H "Content-Type: application/json" \
  -d '{
    "FirstName": "Jane",
    "LastName": "Doe",
    "Company": "ACME Corp",
    "Email": "jane.doe@acme.com",
    "Status": "Qualified"
  }'
```

## Logging

The application provides comprehensive logging at multiple levels:

- **INFO**: Request/response logging, successful operations
- **WARN**: Non-critical issues (e.g., resource not found)
- **ERROR**: Critical errors and failures

Log configuration can be modified in `src/main/resources/log4j2.xml`.

## Security Considerations

1. **Credential Security**: Store credentials securely, consider using secure properties
2. **HTTPS**: Use HTTPS in production environments
3. **Rate Limiting**: Implement rate limiting for production use
4. **Monitoring**: Set up monitoring and alerting for the API
5. **Access Control**: Implement proper access control mechanisms

## Production Deployment

For production deployment:

1. Update Salesforce URLs to production endpoints
2. Configure secure properties for credentials
3. Set up proper SSL/TLS certificates
4. Implement monitoring and logging
5. Configure load balancing and scaling
6. Set up proper backup and disaster recovery

## Troubleshooting

### Common Issues

1. **Authentication Failures**:
   - Verify Salesforce credentials
   - Check security token requirement
   - Ensure Connected App is properly configured

2. **Connection Issues**:
   - Check network connectivity
   - Verify Salesforce URLs
   - Review firewall settings

3. **Performance Issues**:
   - Monitor connection pool usage
   - Review query optimization
   - Check memory usage

### Monitoring Endpoints

- Health Check: `GET /api/v1/health`
- Console: `GET /console`

## Contributing

1. Follow MuleSoft best practices
2. Maintain comprehensive error handling
3. Add appropriate logging
4. Update documentation for changes
5. Test thoroughly with various scenarios

## License

This project is licensed under the MIT License.
