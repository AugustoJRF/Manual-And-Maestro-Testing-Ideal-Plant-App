# Ideal Plant — Accessibility IDs

**Status: Approved by QA**

Accessibility IDs give each element the automated tests need a stable identifier. They must be exposed on iOS as `accessibilityIdentifier` and on Android as `resource-id`. They are used only by the automated tests to find each element. The IDs below are approved and are part of the requirements.

**App ID (iOS bundle ID and Android package name):** `com.idealplant.app`

**Scope:** only the elements needed by the E2E purchase flow (`automation/maestro/suite/e2e_purchase.yaml`). The rest of the app is out of scope for now.

## Naming rules

- Format `screen_element_type`, in lowercase with underscores.
- Elements shared by several screens, like the bottom tab bar, use the prefix `nav` (e.g. `nav_cart_tab`).
- The same ID is used on iOS and Android.
- New IDs need QA approval before they are added to the app. Once approved, an ID is never renamed or removed, because the automated tests depend on it.
- Cards in a list share the same IDs. The ID never includes the product name or number. For testing, the product is selected by position (e.g. `index: 0`).

## IDs

| Screen | Element | ID | Used by |
|---|---|---|---|
| Login | Email field | `login_email_input` | TC-SPL-02, TC-LOG-06 |
| Login | Password field | `login_password_input` | TC-LOG-06 |
| Login | "Log in" button | `login_submit_button` | TC-LOG-06 |
| Shop | "+" of the quantity selector (same ID on every product card) | `shop_quantity_plus_button` | TC-SHOP-04 |
| Shop | "Add to cart" button (same ID on every product card) | `shop_add_to_cart_button` | TC-SHOP-04 |
| Navigation | Cart tab | `nav_cart_tab` | TC-CART-01 |
| Cart | Quantity value of the quantity selector (same ID on every cart item) | `cart_quantity_value` | TC-CART-01 |
| Cart | "Checkout" button | `cart_checkout_button` | TC-CART-07 |
| Checkout | Card payment method | `checkout_card_radio` | TC-CHK-06 |
| Checkout | "Pay" button | `checkout_pay_button` | TC-CART-07, TC-CHK-06 |
| Confirmation | "Continue shopping" button | `confirmation_continue_shopping_button` | TC-CONF-03 |
