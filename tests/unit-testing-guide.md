# Unit Testing Guide

## Overview

This guide outlines the unit testing practices and standards for the Meteorite Drift Bottle project, ensuring code quality and reliability.

## Testing Framework

### Jest Configuration

```typescript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80
    }
  }
};
```

## Test Structure

### Test Organization

```typescript
describe('GameEngine', () => {
  let engine: GameEngine;
  
  beforeEach(() => {
    engine = new GameEngine(mockConfig);
  });
  
  afterEach(() => {
    engine.cleanup();
  });
  
  describe('initialization', () => {
    it('should initialize with correct config', () => {
      expect(engine.config).toEqual(mockConfig);
    });
  });
});
```

## Component Testing

### Entity Tests

```typescript
describe('Entity', () => {
  it('should handle component addition', () => {
    const entity = new Entity();
    const component = new MockComponent();
    entity.addComponent(component);
    expect(entity.getComponent(MockComponent)).toBe(component);
  });
});
```

### System Tests

```typescript
describe('PhysicsSystem', () => {
  it('should update entity positions', () => {
    const system = new PhysicsSystem();
    const entity = createMockEntity();
    system.update([entity]);
    expect(entity.position).toEqual(expectedPosition);
  });
});
```

## Mock Objects

### Creating Mocks

```typescript
const mockRenderer = {
  render: jest.fn(),
  clear: jest.fn(),
  resize: jest.fn()
};

const mockInput = {
  isKeyPressed: jest.fn().mockReturnValue(false),
  update: jest.fn()
};
```

### Using Spies

```typescript
describe('CollisionSystem', () => {
  it('should trigger collision events', () => {
    const spy = jest.spyOn(eventManager, 'emit');
    system.checkCollisions();
    expect(spy).toHaveBeenCalledWith('collision', expect.any(Object));
  });
});
```

## Async Testing

### Promise Testing

```typescript
describe('ResourceLoader', () => {
  it('should load resources asynchronously', async () => {
    const loader = new ResourceLoader();
    await expect(loader.loadTexture('test.png')).resolves.toBeDefined();
  });
});
```

### Timer Mocking

```typescript
describe('Animation', () => {
  beforeEach(() => {
    jest.useFakeTimers();
  });
  
  it('should update frames over time', () => {
    const animation = new Animation();
    animation.start();
    jest.advanceTimersByTime(1000);
    expect(animation.currentFrame).toBe(expectedFrame);
  });
});
```

## Network Testing

### WebSocket Mocking

```typescript
describe('NetworkManager', () => {
  let mockSocket: WebSocket;
  
  beforeEach(() => {
    mockSocket = new MockWebSocket();
    jest.spyOn(window, 'WebSocket').mockImplementation(() => mockSocket);
  });
  
  it('should handle connection events', () => {
    const manager = new NetworkManager();
    manager.connect();
    mockSocket.emit('open');
    expect(manager.isConnected).toBe(true);
  });
});
```

## Performance Testing

### Benchmark Tests

```typescript
describe('Performance', () => {
  it('should handle 1000 entities efficiently', () => {
    const start = performance.now();
    system.update(generateEntities(1000));
    const end = performance.now();
    expect(end - start).toBeLessThan(16); // 60 FPS threshold
  });
});
```

## Coverage Requirements

### Minimum Coverage

- Lines: 80%
- Functions: 80%
- Branches: 80%
- Statements: 80%

## Best Practices

1. Write tests before implementation (TDD)
2. Keep tests focused and isolated
3. Use meaningful test descriptions
4. Maintain test data separately
5. Regular test maintenance

## Common Patterns

### Setup and Teardown

```typescript
describe('TestSuite', () => {
  let testInstance;
  
  beforeAll(() => {
    // One-time setup
  });
  
  beforeEach(() => {
    testInstance = new TestClass();
  });
  
  afterEach(() => {
    testInstance.cleanup();
  });
  
  afterAll(() => {
    // One-time cleanup
  });
});
```

### Error Testing

```typescript
describe('ErrorHandling', () => {
  it('should handle invalid input', () => {
    expect(() => {
      system.process(invalidInput);
    }).toThrow(ValidationError);
  });
});
```

## Continuous Integration

### CI Pipeline

```yaml
# .github/workflows/test.yml
name: Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
```

## Future Improvements

1. Property-based testing
2. Visual regression testing
3. Load testing integration
4. Enhanced mock system
5. Automated test generation