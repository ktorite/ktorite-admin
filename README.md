# Ktorite Admin

[![JitPack](https://jitpack.io/v/ktorite/ktorite-admin.svg)](https://jitpack.io/#ktorite/ktorite-admin)

Admin panel for Ktorite. Auto-generated CRUD for every Exposed table you register, served under `/admin` with Thymeleaf templates and a Catppuccin Latte theme.

This is an internal component of [ktorite-core](https://github.com/ktorite/ktorite) - it has no standalone entry point and depends on core for auth (session validator, superuser check) and model registration. You don't add this dependency yourself; `ktorite-core` bundles it transitively and installs the panel when you call `registerModels(...)`.

## What it gives you

For each registered model:

- Paginated list view with row counts and page navigation
- Detail/edit form generated from the table's columns (booleans become checkboxes, text columns become textareas, auto-increment primary keys are read-only)
- Create and delete actions
- Mass-assignment protection - the primary key is never editable on update

Routes live under `/admin`. Unauthenticated visitors get a login form; the actual credential check is delegated to whatever session auth you configured in ktorite-core (bcrypt + superuser check).

## Security

- Session-gated: everything except `/admin` itself requires an authenticated session, enforced through the validator passed in by ktorite-core
- CSRF: double-submit pattern - a random `csrf_token` cookie scoped to `/admin` must match the `csrf_token` form field on every mutating request

## Usage

Nothing to configure. Add `ktorite-core`, register your models, visit `/admin`:

```kotlin
Ktorite.start {
    // ...
    registerModels(Product, Category)   // these show up in /admin automatically
}
```

Create a superuser first (`./gradlew createsuperuser -Pargs="admin mypassword"`), then log in with those credentials.

## Known limitations

- No search or filtering in list views yet
- No per-model permission system yet (any authenticated superuser can edit anything)

Both are on the roadmap in the main repo.

## License

MIT
