# secure-checkout-action

Secure and version-stable wrapper for [actions/checkout][checkout].

## Description

This action is a wrapper for [actions/checkout][checkout].
By using it, you can fully avoid the need for major version updates of [actions/checkout][checkout].
It allows you to checkout a Git repository at a specific version, similar to [actions/checkout][checkout].
The repository is checked out under `$GITHUB_WORKSPACE`, enabling your workflow to access it.

> [!NOTE]
>
> This action sets `persist-credentials: false` by default to improve security.
> This means the GitHub token will not be saved to `.git/config`, reducing the risk of accidental exposure.
> If your workflow depends on that behavior, set `persist-credentials: true` explicitly.

## Usage

```yaml
  steps:
    - name: Checkout
      uses: tmknom/secure-checkout-action@v1
```

## Inputs and outputs

This action supports the same inputs and outputs as the original [actions/checkout][checkout],
**except** that the default value of `persist-credentials` is set to `false` to improve security.

## Permissions

| Scope    | Access |
| :------- | :----- |
| contents | read   |

## FAQ

### Why do you need this action?

The original [actions/checkout][checkout] gets a major version update every 2-3 years.
Since it's used in most repositories, these updates have a widespread effect.
Every repository that uses [actions/checkout][checkout] must update when a new major version is released.
As the number of repositories increases, this process becomes more time-consuming.
This action helps simplify the upgrade process and reduce effort.

### Why does `actions/checkout` get a major version upgrade every 2-3 years?

This is because [actions/checkout][checkout] depends on Node.js.
Whenever Node.js releases a new major version, [actions/checkout][checkout] must also be updated.
Even LTS versions of Node.js are only supported for up to 3 years,
so when Node.js reaches EOL, [actions/checkout][checkout] needs an upgrade as well.

### Why is a major version upgrade unnecessary for this action?

Major updates for [actions/checkout][checkout] are mostly related to changes in the Node.js runtime.
The interfaces and other backward-compatible features usually stay the same.
For most users, this means that all they have to do is update the version number.
By wrapping [actions/checkout][checkout] in this action, we handle that version change for you.

Additionally, **this action won’t update the major version when the runtime changes**.
By following a different versioning policy from the original [actions/checkout][checkout],
we eliminate the need for major version upgrades. 
Since the interface remains unchanged, users won't be affected even if the original action is released a new major version.

### Why is `persist-credentials` set to false by default?

To improve security, this action disables the default behavior of [actions/checkout][checkout],
which writes the GitHub token to `.git/config`. Disabling this can help prevent accidental reuse
or leakage of credentials in multi-step workflows or shared environments.

If your workflow relies on the original behavior, you can opt in with:

```yaml
with:
  persist-credentials: true
```

## Versioning policy

This action follows a unique versioning policy.
It will never perform major version upgrades.
Raising the major version would negate the purpose of this action.

### Patch version upgrade

When a patch or minor version of [actions/checkout][checkout] is released,
this action applies the patch version upgrade.

### Minor version upgrade

When a major version of [actions/checkout][checkout] is released,
this action applies a minor version upgrade.

### Major version upgrade

Never.

## Release notes

See [GitHub Releases][releases].

[checkout]: https://github.com/actions/checkout
[releases]: https://github.com/tmknom/secure-checkout-action/releases
