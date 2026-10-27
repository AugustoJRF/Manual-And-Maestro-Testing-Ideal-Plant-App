# Review: e2e_purchase (round 3)

**Verdict: Ready for QA approval**

No findings.

| # | Severity | Where | Finding | Suggestion | QA decision |
|---|---|---|---|---|---|

## What was checked (round 3: restructure into commons, scenarios and suite)

- **Matches the instruction:** the suite runs `open_app` → `login` → `shop/add_to_cart` → `cart/review_cart` → `checkout/pay_with_card` → `confirmation/order_placed_successfully`. This covers every step of the QA instruction, in the same order, with no extra steps.
- **Traceability:** every common and scenario starts with one line per test case. All IDs and titles exist in `docs/test-cases.md`: TC-SPL-02, TC-LOG-06, TC-SHOP-04, TC-CART-01, TC-CART-07, TC-CHK-06, TC-CONF-01 and TC-CONF-03. The suite has a 2-line header and there are no other comments.
- **Structure:** reused steps are in `commons/`, area scenarios are in `scenarios/<area>/`, and the runnable test is in `suite/`. The suite contains only `runFlow` calls. Its `../commons/...` and `../scenarios/...` paths are resolved from `suite/` and all point to existing files. Every file has `appId: com.idealplant.app`.
- **Parameters:** `commons/login.yaml` uses `${EMAIL}` and `${PASSWORD}`. The suite defines them in its `env` header with the test user from `docs/test-cases.md` (testing@test.com / Test@1234), and the subflows started with `runFlow` inherit them. `open_app.yaml` has no test data.
- **Selectors and waiting:** all interactions use accessibility IDs. Text is used only to confirm a screen or a message whose exact wording is set by a requirement: "Buy your ideal plant" (SHOP-01) and "Order placed successfully!" (CONF-02). The flow has no fixed waits; the slow steps use `extendedWaitUntil` with a timeout.
- **Accessibility IDs:** the flow uses the 11 IDs listed in `docs/accessibility-ids.md`, and each listed ID is used. The IDs follow the naming format, and the Cart tab uses the `nav` prefix. No approved ID was renamed or removed. The "Scope" line points to the new suite path. The first card is chosen with `index: 0`, and the Cart quantity check (`cart_quantity_value` = 2) confirms the result afterwards.
- **Maestro:** the YAML is valid and uses only existing commands and keys (`launchApp` with `clearState`/`clearKeychain`, `tapOn`, `inputText`, `hideKeyboard`, `extendedWaitUntil`, `runFlow` with `file`, and the `name`/`tags`/`env` config keys). The flow starts from a clean state.
- Settled QA decisions from rounds 1 and 2 were not reopened.

## QA decisions (round 3)

- **Parameters:** the Maestro docs show `${EMAIL}` in `inputText` and passing values with `runFlow` + `env`, but they do not confirm that `env` in the suite header reaches subflows. Changed by the QA: the suite now passes `EMAIL` and `PASSWORD` in the `env` of the `runFlow` that calls `commons/login.yaml`. Agent instructions updated with the same rule.
- **Suite header:** `name` and the tag `e2e` removed. The run command points to the suite file directly, so the tag filtered nothing, and `name` repeated the file name, which Maestro already uses by default.
- **Login file:** `commons/login.yaml` renamed to `commons/login_base.yaml`, to show it is the base login that other suites reuse.
