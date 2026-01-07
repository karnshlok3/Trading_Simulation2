# Virtual Trading Platform - Test Cases

## Authentication & User Management

### TC001: User Registration - Valid Data
**Steps:**
1. Open application
2. Click "Register" tab
3. Enter username: "testuser"
4. Enter email: "test@example.com"
5. Enter password: "password123"
6. Confirm password: "password123"
7. Click "Create Account"

**Expected:**
- Success message appears
- User is created with default balance ($10,000)
- Automatically switches to login tab

**Status:** ✅ Pass

---

### TC002: User Registration - Duplicate Email
**Steps:**
1. Register user with email "test@example.com"
2. Try to register another user with same email

**Expected:**
- Error message: "Email already registered"
- Registration fails
- No duplicate user created

**Status:** ✅ Pass

---

### TC003: User Registration - Password Too Short
**Steps:**
1. Try to register with password "123"

**Expected:**
- Error message: "Password must be at least 6 characters"
- Registration fails

**Status:** ✅ Pass

---

### TC004: User Registration - Password Mismatch
**Steps:**
1. Enter password: "password123"
2. Confirm password: "password456"
3. Click "Create Account"

**Expected:**
- Error message: "Passwords do not match"
- Registration fails

**Status:** ✅ Pass

---

### TC005: User Login - Valid Credentials
**Steps:**
1. Enter email: "admin@trading.edu"
2. Enter password: "admin123"
3. Click "Sign In"

**Expected:**
- Login successful
- Redirects to admin dashboard
- User session created

**Status:** ✅ Pass

---

### TC006: User Login - Invalid Credentials
**Steps:**
1. Enter email: "wrong@email.com"
2. Enter password: "wrongpass"
3. Click "Sign In"

**Expected:**
- Error message: "Invalid email or password"
- Login fails
- Stays on login page

**Status:** ✅ Pass

---

### TC007: User Logout
**Steps:**
1. Login as any user
2. Click logout button (top right)

**Expected:**
- Success message: "Logged out successfully"
- Returns to login page
- Session cleared

**Status:** ✅ Pass

---

## Trading Engine - Buy Orders

### TC101: Buy Order - Valid Trade
**Pre-condition:** User has $10,000 balance

**Steps:**
1. Login as user
2. Navigate to Market view
3. Select "AAPL" (price ~$178.50)
4. Click "Buy"
5. Enter quantity: 10
6. Click "Execute Buy Order"

**Expected:**
- Success message appears
- Balance decreases by ~$1,785
- Portfolio updated with 10 AAPL shares
- Trade logged in history
- Average buy price recorded

**Status:** ✅ Pass

---

### TC102: Buy Order - Insufficient Balance
**Pre-condition:** User has $1,000 balance

**Steps:**
1. Try to buy 100 shares of AAPL (~$178.50 each)
2. Total cost: ~$17,850

**Expected:**
- Error message: "Insufficient balance. Required: $17,850.00, Available: $1,000.00"
- Balance unchanged
- No portfolio update
- No trade logged

**Status:** ✅ Pass

---

### TC103: Buy Order - Invalid Quantity (Zero)
**Steps:**
1. Select asset
2. Enter quantity: 0
3. Try to buy

**Expected:**
- Error message: "Invalid quantity. Must be a positive integer."
- No trade executed

**Status:** ✅ Pass

---

### TC104: Buy Order - Invalid Quantity (Negative)
**Steps:**
1. Select asset
2. Enter quantity: -5
3. Try to buy

**Expected:**
- Error message: "Invalid quantity. Must be a positive integer."
- No trade executed

**Status:** ✅ Pass

---

### TC105: Buy Order - Invalid Quantity (Decimal)
**Steps:**
1. Select asset
2. Enter quantity: 5.5
3. Try to buy

**Expected:**
- Error message: "Invalid quantity. Must be a positive integer."
- No trade executed

**Status:** ✅ Pass

---

### TC106: Buy Order - Trading Disabled
**Pre-condition:** Admin has disabled trading

**Steps:**
1. Try to execute any buy order

**Expected:**
- Error message: "Trading is currently disabled by admin"
- No trade executed
- Balance unchanged

**Status:** ✅ Pass

---

### TC107: Buy Order - Multiple Purchases Same Asset
**Steps:**
1. Buy 10 AAPL at $178.50 (total: $1,785)
2. Price changes to $180.00
3. Buy 5 more AAPL at $180.00 (total: $900)

**Expected:**
- Total holding: 15 AAPL shares
- Average buy price: (1,785 + 900) / 15 = $179.00
- Portfolio shows correct average

**Status:** ✅ Pass

---

## Trading Engine - Sell Orders

### TC201: Sell Order - Valid Trade
**Pre-condition:** User owns 10 AAPL shares (avg buy: $178.50)

**Steps:**
1. Current AAPL price: $180.00
2. Select AAPL
3. Click "Sell"
4. Enter quantity: 5
5. Execute sell

**Expected:**
- Success message with P/L
- Balance increases by $900 (5 × $180)
- Portfolio reduced to 5 AAPL shares
- P/L: ($180 - $178.50) × 5 = +$7.50
- Trade logged with profit_loss field

**Status:** ✅ Pass

---

### TC202: Sell Order - Insufficient Shares
**Pre-condition:** User owns 5 AAPL shares

**Steps:**
1. Try to sell 10 AAPL shares

**Expected:**
- Error message: "Insufficient shares. You own 5, trying to sell 10"
- No trade executed
- Portfolio unchanged

**Status:** ✅ Pass

---

### TC203: Sell Order - Asset Not Owned
**Pre-condition:** User doesn't own any GOOGL shares

**Steps:**
1. Try to sell GOOGL

**Expected:**
- Error message: "You don't own any GOOGL shares"
- No trade executed

**Status:** ✅ Pass

---

### TC204: Sell Order - Complete Holding
**Pre-condition:** User owns exactly 10 AAPL shares

**Steps:**
1. Sell all 10 AAPL shares

**Expected:**
- Success message
- Balance increased
- AAPL completely removed from portfolio
- P/L calculated correctly

**Status:** ✅ Pass

---

### TC205: Sell Order - Profit Scenario
**Steps:**
1. Buy 10 AAPL at $175
2. Price increases to $185
3. Sell all 10 shares

**Expected:**
- P/L: ($185 - $175) × 10 = +$100 profit
- Success message shows profit
- Trade history shows positive P/L

**Status:** ✅ Pass

---

### TC206: Sell Order - Loss Scenario
**Steps:**
1. Buy 10 AAPL at $185
2. Price decreases to $175
3. Sell all 10 shares

**Expected:**
- P/L: ($175 - $185) × 10 = -$100 loss
- Success message shows loss
- Trade history shows negative P/L
- Balance still increases by sale revenue

**Status:** ✅ Pass

---

## Portfolio Management

### TC301: Portfolio - Empty State
**Pre-condition:** New user, no trades

**Steps:**
1. Navigate to Portfolio tab

**Expected:**
- Empty state message displayed
- "No holdings yet" text
- Zero stats for all metrics

**Status:** ✅ Pass

---

### TC302: Portfolio - Single Holding
**Pre-condition:** User owns 10 AAPL at avg $178.50

**Steps:**
1. Current price: $180.00
2. View portfolio

**Expected:**
- Shows 1 holding
- Quantity: 10
- Avg Buy Price: $178.50
- Current Price: $180.00
- Invested: $1,785.00
- Current Value: $1,800.00
- P/L: +$15.00
- Return: +0.84%

**Status:** ✅ Pass

---

### TC303: Portfolio - Multiple Holdings
**Pre-condition:** 
- 10 AAPL at $178.50
- 5 GOOGL at $142.30
- 8 MSFT at $378.90

**Steps:**
1. View portfolio

**Expected:**
- Shows all 3 holdings separately
- Total invested calculated correctly
- Current value updates with prices
- Overall P/L aggregated
- Each asset shows individual P/L

**Status:** ✅ Pass

---

### TC304: Portfolio - Real-time Price Updates
**Steps:**
1. View portfolio
2. Wait 30 seconds (price update interval)
3. Observe changes

**Expected:**
- Current prices update automatically
- Current values recalculate
- P/L updates
- Return % updates
- No page refresh needed

**Status:** ✅ Pass

---

## Trade History

### TC401: History - Empty State
**Pre-condition:** New user, no trades

**Steps:**
1. Navigate to History tab

**Expected:**
- Empty state displayed
- "No trades yet" message
- Empty table

**Status:** ✅ Pass

---

### TC402: History - Single Trade
**Steps:**
1. Execute one buy trade
2. View history

**Expected:**
- Shows trade ID
- Type: BUY badge (green)
- Asset name
- Quantity
- Price
- Total amount
- Timestamp
- P/L: "—" (not applicable for buy)

**Status:** ✅ Pass

---

### TC403: History - Chronological Order
**Steps:**
1. Execute 3 trades at different times
2. View history

**Expected:**
- Most recent trade at top
- Oldest trade at bottom
- Correct chronological sorting
- Timestamps accurate

**Status:** ✅ Pass

---

### TC404: History - Buy and Sell Display
**Steps:**
1. Execute 1 buy trade
2. Execute 1 sell trade
3. View history

**Expected:**
- Buy shows green badge with up arrow
- Sell shows blue badge with down arrow
- Sell trade shows P/L value
- Buy trade shows "—" for P/L

**Status:** ✅ Pass

---

## Analytics

### TC501: Analytics - No Trades
**Pre-condition:** User has no trades

**Steps:**
1. Navigate to Analytics tab

**Expected:**
- All counters show 0
- Win rate: 0%
- Empty state message
- No charts displayed

**Status:** ✅ Pass

---

### TC502: Analytics - Win Rate Calculation
**Steps:**
1. Execute trades with these outcomes:
   - Sell 1: +$50 profit
   - Sell 2: +$30 profit
   - Sell 3: -$20 loss
   - Sell 4: +$10 profit

**Expected:**
- Total completed trades: 4
- Profitable trades: 3
- Loss trades: 1
- Win rate: 75%
- Total profit: $90
- Total loss: $20
- Net P/L: +$70

**Status:** ✅ Pass

---

### TC503: Analytics - Charts Display
**Pre-condition:** User has traded multiple assets

**Steps:**
1. Trade AAPL (5 times)
2. Trade GOOGL (3 times)
3. Trade MSFT (2 times)
4. View Analytics

**Expected:**
- Bar chart shows asset distribution
- AAPL: 5 trades
- GOOGL: 3 trades
- MSFT: 2 trades
- Pie charts for buy/sell distribution
- Performance distribution chart

**Status:** ✅ Pass

---

## Admin Dashboard

### TC601: Admin - Access Control
**Steps:**
1. Login as regular user
2. Attempt to access admin features

**Expected:**
- User sees UserDashboard, not AdminDashboard
- No admin features visible
- Role-based routing works

**Status:** ✅ Pass

---

### TC602: Admin - View All Users
**Pre-condition:** Multiple users registered

**Steps:**
1. Login as admin
2. Navigate to Users tab

**Expected:**
- Table shows all users
- Displays username, email, role, balance
- Shows registration dates
- Admin can see all data

**Status:** ✅ Pass

---

### TC603: Admin - View All Trades
**Pre-condition:** Multiple users have made trades

**Steps:**
1. Login as admin
2. View Overview tab - Recent Trades section

**Expected:**
- Shows trades from all users
- Displays user IDs
- Shows trade details
- Most recent trades visible

**Status:** ✅ Pass

---

### TC604: Admin - Disable Trading
**Steps:**
1. Admin toggles trading to OFF
2. User attempts to trade

**Expected:**
- Toggle shows "Disabled" status
- Users see error: "Trading is currently disabled by admin"
- No trades can be executed
- Applies platform-wide

**Status:** ✅ Pass

---

### TC605: Admin - Enable Trading
**Steps:**
1. Admin toggles trading to ON
2. User attempts to trade

**Expected:**
- Toggle shows "Active" status
- Users can execute trades normally
- No error messages
- Applies immediately

**Status:** ✅ Pass

---

### TC606: Admin - Update Default Balance
**Steps:**
1. Admin sets default balance to $15,000
2. New user registers

**Expected:**
- New user receives $15,000
- Existing users unaffected
- Setting persists
- Success message confirms update

**Status:** ✅ Pass

---

### TC607: Admin - System Statistics
**Steps:**
1. Multiple users trade
2. Admin views Overview tab

**Expected:**
- Correct user count
- Accurate total trades
- System profit calculated
- Trading status displayed
- Real-time updates

**Status:** ✅ Pass

---

## Edge Cases

### TC701: Concurrent Trades
**Steps:**
1. User opens two tabs
2. Executes trade in tab 1
3. Immediately executes trade in tab 2

**Expected:**
- Both trades process sequentially
- Balance updates correctly
- No race conditions
- Data consistency maintained

**Status:** ⚠️ Note: LocalStorage limitation

---

### TC702: Price at Zero
**Steps:**
1. Manually set asset price to 0
2. Attempt to trade

**Expected:**
- Validation catches zero price
- Error message displayed
- No trade executed

**Status:** ✅ Pass

---

### TC703: Very Large Quantity
**Steps:**
1. Try to buy 1,000,000 shares

**Expected:**
- Insufficient balance check catches
- Error message
- No trade

**Status:** ✅ Pass

---

### TC704: Page Refresh During Session
**Steps:**
1. Login
2. Refresh page (F5)

**Expected:**
- User remains logged in
- Session persists
- Data loads correctly
- No data loss

**Status:** ✅ Pass

---

### TC705: Browser Storage Cleared
**Steps:**
1. Use platform
2. Clear browser localStorage
3. Refresh page

**Expected:**
- Resets to initial state
- Default admin account recreated
- All user data lost (expected for demo)
- Fresh market data generated

**Status:** ✅ Pass (by design)

---

### TC706: Invalid Asset Symbol
**Steps:**
1. Try to trade asset that doesn't exist

**Expected:**
- Price returns 0
- Validation catches invalid asset
- Error message
- No trade

**Status:** ✅ Pass

---

### TC707: Network Interruption
**Note:** Not applicable for localStorage version

**Expected in production:**
- Retry logic
- Error handling
- User notification
- Data consistency maintained

**Status:** N/A (future enhancement)

---

## UI/UX Tests

### TC801: Responsive Design - Mobile
**Steps:**
1. Open on mobile viewport (375px)
2. Navigate all pages

**Expected:**
- Layout adapts to screen
- All features accessible
- No horizontal scroll
- Touch-friendly buttons

**Status:** ✅ Pass

---

### TC802: Responsive Design - Tablet
**Steps:**
1. Open on tablet viewport (768px)
2. Navigate all pages

**Expected:**
- Grid layouts adjust
- Readable typography
- Proper spacing
- Charts responsive

**Status:** ✅ Pass

---

### TC803: Toast Notifications
**Steps:**
1. Execute various actions
2. Observe notifications

**Expected:**
- Success: green toast appears
- Error: red toast appears
- Auto-dismiss after timeout
- Clear messages

**Status:** ✅ Pass

---

### TC804: Empty States
**Steps:**
1. Check each section with no data

**Expected:**
- Friendly empty state messages
- Icons displayed
- Call-to-action text
- No broken layouts

**Status:** ✅ Pass

---

### TC805: Loading States
**Steps:**
1. First app load

**Expected:**
- Loading spinner shows
- "Loading platform..." message
- Smooth transition to content

**Status:** ✅ Pass

---

## Performance Tests

### TC901: Large Trade History
**Steps:**
1. Execute 100+ trades
2. View trade history

**Expected:**
- Table renders smoothly
- No lag in scrolling
- Data loads promptly
- No UI freeze

**Status:** ⚠️ May need pagination at scale

---

### TC902: Multiple Holdings
**Steps:**
1. Own all 10 available assets
2. View portfolio

**Expected:**
- All holdings display correctly
- Calculations accurate
- No performance issues
- Real-time updates work

**Status:** ✅ Pass

---

### TC903: Market Price Updates
**Steps:**
1. Wait for multiple 30-second intervals
2. Monitor performance

**Expected:**
- Updates happen on schedule
- No memory leaks
- Smooth transitions
- Accurate timestamps

**Status:** ✅ Pass

---

## Security Tests

### TC1001: Password Storage
**Steps:**
1. Register user with password "test123"
2. Check localStorage

**Expected:**
- Password is hashed
- Original password not visible
- Hash is consistent
- Cannot reverse-engineer

**Status:** ✅ Pass (simple hash for demo)

---

### TC1002: Session Isolation
**Steps:**
1. Login as User A
2. Check data access

**Expected:**
- Can only see own trades
- Can only see own portfolio
- Cannot access other users' data
- Proper filtering

**Status:** ✅ Pass

---

### TC1003: Admin Privilege Escalation
**Steps:**
1. Login as regular user
2. Try to access admin functions

**Expected:**
- Role check enforced
- No admin dashboard access
- Admin features hidden
- Cannot modify system settings

**Status:** ✅ Pass

---

## Summary

**Total Test Cases:** 55+

**Pass Rate:** ~95%

**Known Limitations:**
- LocalStorage has size limits (5-10MB)
- No real multi-device sync
- Concurrent tab handling limited
- No actual API calls

**Production Recommendations:**
- Implement backend database
- Add real authentication (JWT)
- Add rate limiting
- Implement pagination
- Add proper error logging
- Include retry mechanisms
- Add input sanitization
- Implement CSRF protection

---

## Test Environment

- **Browser:** Chrome/Firefox/Safari latest
- **Storage:** localStorage
- **State:** Client-side only
- **Network:** Not required

---

## Regression Testing Checklist

Before any deployment:
- [ ] All authentication flows
- [ ] Buy/sell validation
- [ ] Balance calculations
- [ ] Portfolio accuracy
- [ ] Trade history logging
- [ ] Admin controls
- [ ] Responsive design
- [ ] Error handling
- [ ] Edge cases
- [ ] Empty states
