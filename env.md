# Env

## Managing different env files

1. Create environment files:
```
.env            # default environment
.env.test       # test environment
.env.dev        # development environment
.env.prod       # production environment
```

2. To switch environments, you can use the NODE_ENV variable:

For Mac/Linux:
```bash
# For development
export NODE_ENV=development

# For testing
export NODE_ENV=test

# For production
export NODE_ENV=production
```

3. In your code, use a package like `dotenv` to load the correct file:

```javascript
require('dotenv').config({
  path: `.env.${process.env.NODE_ENV || 'development'}`
});
```

For testing specifically:
```bash
NODE_ENV=test npm test
```
