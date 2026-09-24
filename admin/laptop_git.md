# Laptop Git Auth

Programming laptops use a Personal Access Token (PAT) for credential management.

To add or update a token:
1. Log in as the DRSS Robotics bot user.
2. Go to [Tokens (classic)](https://github.com/settings/tokens) in Settings.
3. "Generate new token (classic)"
    * Note should identify the laptop (e.g. `laptop-10011-3-darkforest`)
    * Expiration: recommended setting to the end of the school year (e.g. 1 June 2027)
    * Scopes: "repo"
4. Open the Windows Credential Manager -> "Windows Credentials" -> "Generic Credentials"
5. Add or update the GitHub credential.
    * Internet or network address: `git:https://github.com`
    * User name: `<bot-user>`
    * Password: `<the PAT generated above>`

Tokens are set to expire at the end of each school year.

## Git cheat sheet

GitHub auth for pushes is different from the Git user settings when writing commits.  Ensure Git is properly configured to attribute commits correctly:

```sh
git config user.name "Your name"
git config user.email "10011@example.org"
```

Use `git config --global` for global scope.
