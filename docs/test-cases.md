# Ideal Plant — Manual Test Cases

Manual test cases for the Ideal Plant app (iOS and Android), written in Given / When / Then.
Each test case lists the requirements it covers (see [requirements.md](requirements.md)).

## Common preconditions

These apply to every test case and are not repeated in each one:

- The app is installed on an iOS or Android device.
- The device has an internet connection.
- Tests run in the test environment, where the development team provides fake product data (seed data) through the back office. The stock values each test needs are set up there before the test runs.
- In the tests, plants are called plant1, plant2… because any plant from the test data can be used. The plant names in the screens are just examples.
- The test user exists:

| Field | Value |
|---|---|
| Name | User Testing |
| Email | testing@test.com |
| Password | Test@1234 |
| Delivery address | Calle Testing, 00000 A Coruña, Spain |

- Unless a test says otherwise, the cart and Favorites start empty.

## Summary

Each test case shows the requirements it covers, its priority and whether it is a candidate for automation. Being a candidate does not mean it will be automated: that decision is made in the automation phase.

| Screen | Test cases | High priority | Automation candidates |
|---|---|---|---|
| Splash | 2 | 1 | 1 |
| Login | 6 | 2 | 5 |
| Shop | 6 | 4 | 6 |
| Favorites | 6 | 1 | 4 |
| Cart | 7 | 3 | 6 |
| Checkout | 6 | 3 | 6 |
| Confirmation | 4 | 4 | 4 |
| Account | 3 | 2 | 3 |
| **Total** | **40** | **20** | **35** |

---

## Splash

**TC-SPL-01 · Splash screen content**<br>
Requirements: SPL-01, NAV-01 · Priority: Medium · Automation candidate: No

```gherkin
Given the app is closed
When the user opens the app
Then the splash screen is shown on a light green background
And the Ideal Plant logo is displayed
And the name "Ideal Plant" is displayed
And the tagline "Buy your ideal plant" is displayed
And the bottom tab bar is not shown
```

**TC-SPL-02 · Splash opens the right screen on its own**<br>
Requirements: SPL-02, LOG-07, ACC-02 · Priority: High · Automation candidate: Yes

```gherkin
Given the user <session>
And the app is closed
When the user opens the app
And waits without touching the screen
Then the <screen> screen is shown after about 2 seconds

Examples:
  | session                                        | screen |
  | has never logged in                            | Login  |
  | logged in and closed the app                   | Shop   |
  | logged in, tapped "Log out" and closed the app | Login  |
```

> On slower devices the next screen may take longer to appear. This is not a failure as long as it appears on its own.

---

## Login

**TC-LOG-01 · Login screen content**<br>
Requirements: LOG-01, NAV-01 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Login screen
When the user views the screen
Then the Email field is displayed
And the Password field is displayed
And the "Forgot password?" link is displayed
And the "Log in" button is displayed
And the bottom tab bar is not shown
```

**TC-LOG-02 · "Log in" is disabled while a field is empty**<br>
Requirements: LOG-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Login screen
When the Email field contains <email>
And the Password field contains <password>
Then the "Log in" button is disabled

Examples:
  | email            | password  |
  | (empty)          | (empty)   |
  | testing@test.com | (empty)   |
  | (empty)          | Test@1234 |
```

**TC-LOG-03 · "Log in" is enabled when both fields have text**<br>
Requirements: LOG-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Login screen
When the user enters "testing@test.com" in the Email field
And enters "Test@1234" in the Password field
Then the "Log in" button is enabled
```

**TC-LOG-04 · Invalid login data shows the right error message**<br>
Requirements: LOG-03, LOG-04 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Login screen
When the user enters <email> in the Email field
And enters <password> in the Password field
And taps "Log in"
Then the "<message>" message is shown <position>
And the user stays on the Login screen

Examples:
  | email                   | password   | message              | position                 |
  | testing@test            | Test@1234  | Invalid email format | under the Email field    |
  | testing@test.com        | Wrong@1234 | Invalid credentials  | above the Email field    |
  | not.registered@test.com | Test@1234  | Invalid credentials  | above the Email field    |
```

> Rows 2 and 3 must show exactly the same message, so the app never reveals whether the email or the password is wrong.

**TC-LOG-05 · Show and hide the password**<br>
Requirements: LOG-06 · Priority: Low · Automation candidate: No

```gherkin
Given the user is on the Login screen
And "Test@1234" is entered in the Password field
And the password is <initial state>
When the user taps "<button>"
Then the password is <final state>
And the button reads "<new label>"

Examples:
  | initial state       | button | final state | new label |
  | hidden (by default) | Show   | visible     | Hide      |
  | visible             | Hide   | hidden      | Show      |
```

**TC-LOG-06 · Successful login opens the Shop**<br>
Requirements: LOG-05 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Login screen
When the user enters "testing@test.com" in the Email field
And enters "Test@1234" in the Password field
And taps "Log in"
Then the Shop screen is shown
```

---

## Shop

All Shop test cases start with the user logged in.

**TC-SHOP-01 · Shop screen content**<br>
Requirements: SHOP-01, SHOP-02, SHOP-03, NAV-01, NAV-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Shop screen
When the user views the screen
Then the title "Buy your ideal plant" is displayed
And four product cards are visible
And each product card shows the photo, name, price, stock, heart icon, quantity selector and "Add to cart" button
And the bottom tab bar is shown with the Shop tab highlighted
```

**TC-SHOP-02 · Stock label and button state follow the stock**<br>
Requirements: SHOP-05, SHOP-06 · Priority: High · Automation candidate: Yes

```gherkin
Given <plant> has a stock of <stock>
And the user is on the Shop screen
When the user views the card of <plant>
Then the stock label reads "<stock label>"
And the selector shows <selector>
And the button reads "<button>" and is <button state>

Examples:
  | plant  | stock | stock label  | selector | button      | button state |
  | plant1 | 4     | 4 in stock   | 1        | Add to cart | enabled      |
  | plant2 | 3     | Only 3 left  | 1        | Add to cart | enabled      |
  | plant3 | 1     | Only 1 left  | 1        | Add to cart | enabled      |
  | plant4 | 0     | Out of stock | 0        | Unavailable | disabled     |
```

**TC-SHOP-03 · Quantity selector limits**<br>
Requirements: SHOP-04, SHOP-08 · Priority: High · Automation candidate: Yes

```gherkin
Given a plant has a stock of 8
And the cart already contains <in cart> of that plant
And the user is on the Shop screen
When the user sets the quantity of that plant to <quantity>
Then "−" is <minus>
And "+" is <plus>

Examples:
  | in cart | quantity | minus    | plus     |
  | 0       | 1        | disabled | enabled  |
  | 0       | 4        | enabled  | enabled  |
  | 0       | 8        | enabled  | disabled |
  | 3       | 5        | enabled  | disabled |
  | 7       | 1        | disabled | disabled |
```

> The last two rows check that the maximum takes the cart into account: 8 in stock minus what is already in the cart.

**TC-SHOP-04 · "Add to cart" updates the cart and resets the selector**<br>
Requirements: SHOP-07, NAV-03 · Priority: High · Automation candidate: Yes

```gherkin
Given a plant has a stock of 5
And the cart is empty
And the user is on the Shop screen
When the user sets the quantity of that plant to 2
And taps "Add to cart" on that plant's card
Then the Cart tab shows 2
And the selector of that plant shows 1
And the stock label still reads "5 in stock"
```

**TC-SHOP-05 · All stock already in the cart blocks adding more**<br>
Requirements: SHOP-09 · Priority: High · Automation candidate: Yes

```gherkin
Given a plant has a stock of 3
And the cart already contains 3 of that plant
When the user <navigation>
Then the selector of that plant shows 0
And the button reads "All in your cart" and is disabled

Examples:
  | navigation                                      |
  | opens the Shop                                  |
  | opens the Cart tab and then returns to the Shop |
```

**TC-SHOP-06 · Add and remove a favorite with the heart**<br>
Requirements: SHOP-10 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Shop screen
And the heart of a plant is <initial heart>
When the user taps that heart
Then the heart becomes <final heart>
And the plant <favorites result> on the Favorites screen

Examples:
  | initial heart | final heart | favorites result |
  | empty         | filled      | is shown         |
  | filled        | empty       | is not shown     |
```

---

## Favorites

All Favorites test cases start with the user logged in.

**TC-FAV-01 · Favorites tab shows the favorite plants**<br>
Requirements: FAV-01, NAV-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given plant1 and plant2 are favorites
And the user is on the Shop screen
When the user taps the Favorites tab
Then the Favorites screen is shown with the Favorites tab highlighted
And plant1 and plant2 are listed with a filled heart
And their cards show the same information and follow the same rules as in the Shop
```

**TC-FAV-02 · Remove a plant from Favorites**<br>
Requirements: FAV-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given plant1 and plant2 are favorites
And the user is on the Favorites screen
When the user taps the filled heart of plant1
Then plant1 is no longer listed
And plant2 is still listed
```

**TC-FAV-03 · An out-of-stock plant can be a favorite**<br>
Requirements: FAV-03 · Priority: Low · Automation candidate: No

```gherkin
Given a plant has a stock of 0
And the user is on the Shop screen
When the user taps the heart of that plant
And opens the Favorites tab
Then the plant is listed
And its card shows "Out of stock"
```

**TC-FAV-04 · Favorites are kept after reopening the app**<br>
Requirements: FAV-04 · Priority: High · Automation candidate: Yes

```gherkin
Given plant1 is a favorite
When the user closes the app
And opens it again
And opens the Favorites tab
Then plant1 is listed with a filled heart
```

**TC-FAV-05 · Empty Favorites screen content**<br>
Requirements: FAV-05 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user has no favorites
When the user opens the Favorites tab
Then the message "No favorites yet" is displayed
And the "Browse plants" button is displayed
```

**TC-FAV-06 · "Browse plants" opens the Shop**<br>
Requirements: FAV-05 · Priority: Low · Automation candidate: No

```gherkin
Given the user has no favorites
And the user is on the Favorites screen
When the user taps "Browse plants"
Then the Shop screen is shown
```

---

## Cart

All Cart test cases start with the user logged in. Prices and stock values are test data, not requirements.

**TC-CART-01 · Cart screen content**<br>
Requirements: CART-01, NAV-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the cart contains 2 of plant1
And the user is on the Shop screen
When the user taps the Cart tab
Then the Cart screen is shown with the Cart tab highlighted
And plant1 shows its photo, name, unit price, quantity selector with 2, line total and trash icon
And the "Checkout" button is displayed
```

**TC-CART-02 · Quantity selector limits in the cart**<br>
Requirements: CART-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given plant1 has a stock of 3
And the cart contains <quantity> of plant1
When the user opens the Cart tab
Then "−" of plant1 is <minus>
And "+" of plant1 is <plus>

Examples:
  | quantity | minus    | plus     |
  | 1        | disabled | enabled  |
  | 2        | enabled  | enabled  |
  | 3        | enabled  | disabled |
```

**TC-CART-03 · Totals update when the quantity changes**<br>
Requirements: CART-03, NAV-03 · Priority: High · Automation candidate: Yes

```gherkin
Given plant1 costs €10.00
And the cart contains 1 of plant1
And the user is on the Cart screen
When the user taps "+" of plant1
Then the line total of plant1 is €20.00
And the total shows 2 items and €20.00
And the Cart tab shows 2
```

**TC-CART-04 · Removing the last product shows the empty cart**<br>
Requirements: CART-02, CART-03, CART-04, NAV-03 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the cart contains only plant1
And the user is on the Cart screen
When the user taps the trash icon of plant1
Then plant1 is removed without a confirmation step
And the message "Your cart is empty" is displayed
And the "Browse plants" button is displayed
And the Cart tab shows no number
```

**TC-CART-05 · "Browse plants" opens the Shop**<br>
Requirements: CART-04 · Priority: Low · Automation candidate: No

```gherkin
Given the cart is empty
And the user is on the Cart screen
When the user taps "Browse plants"
Then the Shop screen is shown
```

**TC-CART-06 · The cart is kept after reopening the app**<br>
Requirements: CART-06 · Priority: High · Automation candidate: Yes

```gherkin
Given the cart contains 2 of plant1
When the user closes the app
And opens it again
And opens the Cart tab
Then plant1 is listed with a quantity of 2
And the Cart tab shows 2
```

**TC-CART-07 · "Checkout" opens the Checkout screen**<br>
Requirements: CART-05 · Priority: High · Automation candidate: Yes

```gherkin
Given the cart contains 1 of plant1
And the user is on the Cart screen
When the user taps "Checkout"
Then the Checkout screen is shown
```

---

## Checkout

All Checkout test cases start with the user logged in and at least one plant in the cart. Prices are test data, not requirements.

**TC-CHK-01 · Checkout screen content**<br>
Requirements: CHK-01, NAV-01 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Cart screen
When the user taps "Checkout"
Then the delivery address is the same as on the Account screen, with an "Edit" link
And the payment methods Card, PayPal, Bizum and Cash on delivery are displayed
And no payment method is selected
And the order total is displayed
And the bottom tab bar is not shown
```

**TC-CHK-02 · "Pay" is enabled only after choosing a payment method**<br>
Requirements: CHK-03 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Checkout screen
When the user <selection>
Then the "Pay" button is <state>

Examples:
  | selection                        | state    |
  | does not select a payment method | disabled |
  | selects Card                     | enabled  |
```

**TC-CHK-03 · Only one payment method can be selected**<br>
Requirements: CHK-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Checkout screen
And Card is selected
When the user selects PayPal
Then PayPal is selected
And Card is no longer selected
```

**TC-CHK-04 · "Pay" shows the cart total**<br>
Requirements: CHK-04 · Priority: High · Automation candidate: Yes

```gherkin
Given plant1 costs €10.00
And the cart contains 2 of plant1
And the user is on the Cart screen
When the user taps "Checkout"
Then the order total is €20.00
And the button reads "Pay €20.00"
```

**TC-CHK-05 · Going back returns to the Cart with its content**<br>
Requirements: CHK-06 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the cart contains 2 of plant1
And the user is on the Checkout screen
When the user goes back with <back action>
Then the Cart screen is shown
And plant1 is listed with a quantity of 2

Examples:
  | back action                               |
  | the back arrow                            |
  | the Android system back (button or swipe) |
  | the iOS swipe back gesture                |
```

**TC-CHK-06 · Paying with each method opens the Confirmation**<br>
Requirements: CHK-05 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Checkout screen
When the user selects <payment method>
And taps "Pay"
Then the Confirmation screen is shown

Examples:
  | payment method   |
  | Card             |
  | PayPal           |
  | Bizum            |
  | Cash on delivery |
```

---

## Confirmation

All Confirmation test cases start with the user logged in, at least one plant in the cart and a payment method selected on the Checkout screen.

**TC-CONF-01 · Payment is confirmed and the order is placed**<br>
Requirements: CONF-01, CONF-02, NAV-01 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Checkout screen
When the user taps "Pay"
Then "Confirming your payment…" is displayed while the payment is processed
And "Order placed successfully!" is displayed once it is confirmed
And the screen says the order details were sent by email
And an order number is displayed
And the bottom tab bar is not shown
```

> The processing state can be very short. Wait for the success message instead of a fixed time.

**TC-CONF-02 · The user cannot go back to Checkout**<br>
Requirements: CONF-03 · Priority: High · Automation candidate: Yes

```gherkin
Given the user tapped "Pay"
And the "<state>" screen is shown
When the user tries to go back with <back action>
Then the Checkout screen is not shown

Examples:
  | state                      | back action                               |
  | Confirming your payment…   | the Android system back (button or swipe) |
  | Confirming your payment…   | the iOS swipe back gesture                |
  | Order placed successfully! | the Android system back (button or swipe) |
  | Order placed successfully! | the iOS swipe back gesture                |
```

**TC-CONF-03 · "Continue shopping" opens the Shop with the updated stock**<br>
Requirements: CONF-05, CONF-06 · Priority: High · Automation candidate: Yes

```gherkin
Given plant1 has a stock of 5
And the cart contained 1 of plant1
And the order was placed
And "Order placed successfully!" is displayed
When the user taps "Continue shopping"
Then the Shop screen is shown
And the stock label of plant1 reads "4 in stock"
```

> The stock is updated by the Product & Stock API after the payment is confirmed. Wait for the update before checking the stock label; how long it takes is out of scope.

**TC-CONF-04 · The cart is empty after a successful order**<br>
Requirements: CONF-04, NAV-03 · Priority: High · Automation candidate: Yes

```gherkin
Given the order was placed
And "Order placed successfully!" is displayed
When the user taps "Continue shopping"
And opens the Cart tab from the Shop screen
Then the message "Your cart is empty" is displayed
And the Cart tab shows no number
```

---

## Account

All Account test cases start with the user logged in.

**TC-ACC-01 · Account screen content**<br>
Requirements: ACC-01, NAV-02 · Priority: Medium · Automation candidate: Yes

```gherkin
Given the user is on the Shop screen
When the user taps the Account tab
Then the Account screen is shown with the Account tab highlighted
And the test user's initials, name and email are displayed
And the delivery address is displayed with an "Edit" link
And the "Log out" button is displayed
```

**TC-ACC-02 · "Log out" opens the Login screen**<br>
Requirements: ACC-02 · Priority: High · Automation candidate: Yes

```gherkin
Given the user is on the Account screen
When the user taps "Log out"
Then the Login screen is shown
```

**TC-ACC-03 · Going back after logging out does not reopen the account**<br>
Requirements: ACC-02 · Priority: High · Automation candidate: Yes

```gherkin
Given the user tapped "Log out"
And the Login screen is shown
When the user tries to go back with <back action>
Then no screen of the logged-in user is shown

Examples:
  | back action                               |
  | the Android system back (button or swipe) |
  | the iOS swipe back gesture                |
```
