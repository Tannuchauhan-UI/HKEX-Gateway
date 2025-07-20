# HKEX Trading Gateway

A C++ implementation of a trading gateway for connecting to the Hong Kong Exchange (HKEX) using the OMnet API.

## Features

- Full integration with HKEX OMnet API
- Order management (submit/modify/cancel)
- Market data handling
- Trade management
- Session management
- Configurable logging
- Thread-safe implementation
- JSON-based configuration
- Callback-based event notifications

## Prerequisites

- C++17 compatible compiler
- CMake 3.10 or higher
- OpenSSL
- Boost (system, thread)
- JsonCpp
- OMnet API headers and libraries

## Building

1. Create a build directory:
```bash
mkdir build && cd build
```

2. Configure with CMake:
```bash
cmake ..
```

3. Build:
```bash
make
```

## Configuration

Create a configuration file based on the template in `config/gateway.json`:

```json
{
    "connection": {
        "host": "localhost",
        "port": 8080,
        "username": "your_username",
        "password": "your_password"
    },
    "logging": {
        "log_file": "logs/gateway.log",
        "error_file": "logs/error.log",
        "log_level": "INFO"
    },
    "market_data": {
        "symbols": [
            "HSI",
            "HSCEI",
            "MHI"
        ],
        "subscription_mode": "snapshot",
        "update_interval_ms": 1000
    }
}
```

## Usage

1. Start the gateway:
```bash
./gateway_main /path/to/config.json
```

2. The gateway will:
   - Initialize logging
   - Connect to HKEX
   - Subscribe to configured market data
   - Start processing messages
   - Handle orders and trades

3. To stop the gateway, press Ctrl+C for graceful shutdown

## Architecture

### Core Components

1. **HKEXGateway**
   - Main gateway class
   - Coordinates all components
   - Handles initialization and shutdown
   - Manages message processing thread

2. **SessionHandler**
   - Manages OMnet API sessions
   - Handles login/logout
   - Maintains connection state

3. **OrderManager**
   - Handles order submissions
   - Manages order modifications
   - Processes order cancellations
   - Tracks order status

4. **MarketDataHandler**
   - Subscribes to market data
   - Processes market data updates
   - Maintains latest market data
   - Notifies subscribers

5. **TradeManager**
   - Processes trade confirmations
   - Maintains trade history
   - Links trades to orders
   - Provides trade queries

6. **Logger**
   - Thread-safe logging
   - Multiple log levels
   - Timestamp-based logging
   - File and console output

7. **ErrorHandler**
   - Centralizes error handling
   - Provides error messages
   - Throws appropriate exceptions
   - Logs error details

### Message Flow

1. Incoming Messages:
   ```
   HKEX → OMnet API → Gateway → Component Handlers → Callbacks
   ```

2. Outgoing Messages:
   ```
   Client → Gateway → OrderManager → OMnet API → HKEX
   ```

## Error Handling

The gateway implements comprehensive error handling:

1. OMnet API errors
2. Network connectivity issues
3. Invalid messages
4. Configuration errors
5. System errors

All errors are:
- Logged with details
- Propagated to appropriate handlers
- Notified to clients when necessary

## Thread Safety

The gateway is designed to be thread-safe:

1. Singleton instances with thread-safe initialization
2. Mutex protection for shared resources
3. Thread-safe logging
4. Atomic operations for status flags
5. Message queue for processing

## Best Practices

1. Always use the configuration file
2. Monitor the log files
3. Implement proper error handling in callbacks
4. Use appropriate order types
5. Handle reconnection scenarios
6. Regular status checks

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support:
1. Check the documentation
2. Review the logs
3. Contact HKEX support for API-specific issues
4. Open an issue for bugs or feature requests 