# Review: e2e_purchase (round 1)

> This round reviewed the first version of the flow: a single file, `automation/maestro/flows/e2e_purchase.yaml`. In round 3 it was split into `commons/`, `scenarios/` and `suite/`, so the line numbers below refer to that old file.

**Verdict: Changes needed**

| # | Severity | Where | Finding | Suggestion | QA decision |
|---|---|---|---|---|---|
| 1 | Major | `e2e_purchase.yaml` line 15-16 | `clearState: true` does not clear the iOS keychain in Maestro. LOG-07 keeps the user logged in after reopening; if the app stores the session in the keychain, the iOS run opens the Shop instead of Login (SPL-02) and the wait on line 20 fails. The flow may pass on Android and fail on iOS. | Add `clearKeychain: true` to `launchApp` (iOS only, ignored on Android). | **Accepted.** The session must not survive a clean start on iOS. |
| 2 | Major | `e2e_purchase.yaml` line 54-67 | Nothing confirms that the tap on line 54 added exactly one plant. The check on line 64 only proves the cart is not empty, and that only means something if the cart was empty at the start. `clearState` clears local data, but what the cart holds after logging in again is out of scope in `requirements.md`, so a cart kept on the server is not excluded. In that case the flow would buy more than one plant and still pass. | Add a check after line 54 that the cart holds 1 unit (e.g. the Cart tab number, NAV-03, which needs a new proposed ID), or have the QA confirm that the test data setup empties the test user's cart before the run. | **Accepted, with a change.** Instead of checking the Cart tab, the flow adds 2 units with the "+" button and checks the quantity 2 on the Cart screen, following TC-SHOP-04 and TC-CART-01. |

## Other QA decisions (round 1)

- The Cart tab ID is renamed from `tabbar_cart_tab` to `nav_cart_tab`. Elements shared by several screens, like the bottom tab bar, use the prefix `nav`. The IDs were still a proposal, so the "never rename an ID" rule does not apply yet.
- The first card is chosen by position (`index: 0`). Product IDs are not used in accessibility IDs, because they can change. The quantity check on the Cart screen protects the result: if the wrong card is used, the flow fails.
- Comments in the flow: a header of up to 3 lines, and one line with the test case ID and title above the commands of each test case. No other comments.
- The app id is added to `docs/accessibility-ids.md`, because the developers need it too.
