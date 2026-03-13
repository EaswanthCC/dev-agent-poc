# Salesforce Account SAPI - Optimized

## Overview

This MuleSoft application implements a System API (SAPI) for Salesforce Account Object operations following the API-led connectivity approach. It provides a comprehensive set of RESTful endpoints for managing Salesforce Account data with proper error handling, logging, and monitoring capabilities.

## Features

- **CRUD Operations**: Complete Create, Read, Update operations for Salesforce Accounts
- **RAML-First Design**: API specification defined using RAML 1.0 with comprehensive documentation
- **DataWeave Transformations**: Proper field mapping between Salesforce and API formats
- **Error Handling**: Comprehensive error handling with meaningful error responses
- **Health Check**: Built-in health monitoring endpoint
- **Security**: Secure properties configuration for sensitive data
- **Testing**: Complete MUnit test suite with mocking capabilities
- **Deployment Ready**: CloudHub deployment configuration included

## API Endpoints

### Account Operations
- `GET /api/accounts` - Retrieve accounts with optional filtering
- `GET /api/accounts/{id}` - Retrieve account by ID
- `POST /api/accounts` - Create new account
- `PATCH /api/accounts/{id}` - Update existing account

### Health Check
- `GET /api/health` - Health status and Salesforce connectivity check

## Supported Account Fields

The API supports the following Salesforce Account object fields:
- AccountId, AccountName, AccountNumber
- Type, Industry
- BillingAddress (street, city, state, postalCode, country)
- ShippingAddress (street, city, state, postalCode, country)
- Phone, Website
- OwnerId, CreatedDate, LastModifiedDate

## Configuration

### Properties Files
- `config.properties` - General configuration properties
- `secure-config.properties` - Encrypted sensitive properties

### Key Configuration Properties
```properties
# Salesforce Configuration
sfdc.username=your_username
sfdc.password=your_password
sfdc.token=your_security_token
sfdc.url=https://login.salesforce.com/services/Soap/u/60.0

# HTTP Configuration
http.port=8081
http.host=0.0.0.0

# API Management
api.id=your_api_id
api.version=v1.0.0
```

## Project Structure

```
3-salesforce-sapi-Optimized/
├── src/
│   ├── main/
│   │   ├── mule/
│   │   │   ├── 3-salesforce-sapi-optimized.xml  # Main flows
│   │   │   └── global.xml                       # Global configurations
│   │   └── resources/
│   │       ├── api/
│   │       │   └── salesforce-account-sapi.raml # API specification
│   │       ├── config.properties                # Configuration
│   │       ├── secure-config.properties         # Secure properties
│   │       └── log4j2.xml                      # Logging configuration
│   └── test/
│       └── munit/
│           └── 3-salesforce-sapi-optimized-test-suite.xml # Unit tests
├── pom.xml                                      # Maven configuration
└── README.md                                    # This file
```

## Dependencies

### Core Connectors
- **Salesforce Connector**: v10.20.0 - For Salesforce operations
- **HTTP Connector**: v1.9.3 - For REST API endpoints
- **APIKit Module**: v1.11.1 - For RAML-based API implementation

### Security & Configuration
- **Secure Configuration Property Module**: v1.2.7 - For encrypted properties
- **API Gateway**: v1.6.0 - For API management and auto-discovery

### Testing
- **MUnit Runner**: v3.5.0 - For unit testing
- **MUnit Tools**: v3.5.0 - For mocking and assertions

## Getting Started

### Prerequisites
1. Anypoint Studio 7.x or Anypoint Code Builder
2. Mule Runtime 4.11.0
3. Maven 3.6+
4. Salesforce Developer/Sandbox org with API access
5. Java 17

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd 3-salesforce-sapi-Optimized
   ```

2. **Configure Salesforce Connection**
   - Update `config.properties` with your Salesforce credentials
   - For production, encrypt sensitive values and use `secure-config.properties`

3. **Install Dependencies**
   ```bash
   mvn clean install
   ```

4. **Run Tests**
   ```bash
   mvn test
   ```

5. **Run Application Locally**
   ```bash
   mvn mule:run
   ```

6. **Access API**
   - Base URL: `http://localhost:8081/api`
   - Health Check: `http://localhost:8081/api/health`

## Deployment

### CloudHub Deployment
The application is configured for CloudHub deployment with the following settings:
- **Runtime**: Mule 4.11.0
- **Application Name**: salesforce-account-sapi-optimized
- **Environment**: Configurable (dev/qa/prod profiles)

Deploy using Maven:
```bash
mvn deploy -DmuleDeploy
```

### Environment Configuration
Use Maven profiles for different environments:
- **Development**: `mvn mule:run -Pdev`
- **QA**: `mvn mule:run -Pqa`
- **Production**: `mvn mule:run -Pprod`

## API Management

### Auto-Discovery
The application supports Anypoint API Manager auto-discovery:
1. Set `api.id` and `api.version` in properties
2. Configure client credentials for platform access
3. Deploy to enable automatic API registration

### Monitoring
- Built-in health check endpoint for monitoring
- Comprehensive logging with correlation IDs
- Error tracking and performance metrics
- MUnit tests with coverage reporting

## Error Handling

The API provides standardized error responses:

```json
{
  "code": "ERROR_CODE",
  "message": "Detailed error description",
  "timestamp": "2024-03-13T19:15:00Z",
  "correlationId": "correlation-id"
}
```

### Error Codes
- `BAD_REQUEST`: Invalid request format or parameters
- `NOT_FOUND`: Resource not found
- `ACCOUNT_NOT_FOUND`: Specific account not found
- `ACCOUNT_CREATION_FAILED`: Account creation failed
- `ACCOUNT_UPDATE_FAILED`: Account update failed
- `SFDC_ERROR`: Salesforce-specific errors
- `INTERNAL_ERROR`: Unexpected system errors

## Testing

### MUnit Tests
Comprehensive test suite covering:
- GET /accounts with various parameters
- GET /accounts/{id} success and not found scenarios
- POST /accounts account creation
- PATCH /accounts/{id} account updates
- GET /health connectivity checks

Run tests:
```bash
mvn test
```

Generate coverage report:
```bash
mvn munit:coverage-report
```

### Test Coverage
- Target coverage: 75% application coverage
- Minimum flow coverage: 50%
- Generates HTML and JSON reports

## Best Practices Implemented

### MuleSoft Best Practices
- **API-Led Connectivity**: System API layer implementation
- **RAML-First Design**: Complete API specification with examples
- **Error Handling**: Comprehensive error handling strategy
- **Configuration Management**: Externalized configuration
- **Security**: Encrypted sensitive properties
- **Logging**: Structured logging with correlation IDs
- **Testing**: Complete unit test coverage

### Salesforce Integration
- **SOQL Optimization**: Efficient query construction
- **Field Mapping**: Complete Salesforce to API field mapping
- **Null Handling**: Proper handling of null/empty values
- **Connection Management**: Robust connection configuration

## Troubleshooting

### Common Issues

1. **Salesforce Connection Issues**
   - Verify username, password, and security token
   - Check Salesforce IP restrictions
   - Validate API version compatibility

2. **API Kit Issues**
   - Ensure RAML file is valid
   - Check flow naming conventions match RAML operations
   - Verify all required dependencies are included

3. **Deployment Issues**
   - Check CloudHub application name uniqueness
   - Verify environment-specific properties
   - Ensure proper Maven plugin configuration

### Debugging
- Enable DEBUG logging for detailed flow execution
- Use Anypoint Studio debugger for local testing
- Monitor CloudHub logs for deployment issues

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes following MuleSoft best practices
4. Add/update tests as needed
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions:
- Review MuleSoft documentation
- Check Salesforce API documentation
- Contact the development team

---

**Version**: 1.0.0  
**Last Updated**: March 13, 2026  
**Mule Runtime**: 4.11.0  
**API Version**: v1.0.0
