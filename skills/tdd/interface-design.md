# Interface Design for Testability

Good interfaces make testing natural:

1. **Accept dependencies, don't create them**

   ```typescript
   // Testable
   function processOrder(order, paymentGateway) {}

   // Hard to test
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. **Prefer return values when they fit; verify side effects through public behavior**

   ```typescript
   // Testable pure query
   function calculateDiscount(cart): Discount {}

   // Testable command: side effect is observed through a public interface
   function applyDiscount(cartId): void {}
   function getCart(cartId): Cart {}
   ```

   Do not reshape a command-style public API into a query just for test convenience. For side-effectful behavior, verify the observable result through a public query, event, API response, CLI output, or user-visible flow instead of direct hidden-state inspection.

3. **Small surface area**
   - Fewer methods = fewer tests needed
   - Fewer params = simpler test setup
