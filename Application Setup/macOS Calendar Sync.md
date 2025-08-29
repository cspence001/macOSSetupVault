##### This worked when attempting to add Google Account to macOS Calendar App, and gets stuck loading after selecting Google mail server:
##### Manual Configuration via CalDAV (Advanced)
If the graphical methods are completely broken, you can add _just the calendar_ functionality using the older CalDAV protocol. (Note: This will only add Calendar, not Mail or Contacts).

1. Open the **Calendar** app.
2. From the menu bar, go to **Calendar > Add Account...**.
3. **Do not click "Google"**. Instead, select **Other Calendar Account...** at the bottom.
4. Click **Continue**.
5. Fill in the details as follows:
    - **Account Type:** CalDAV
    - **User Name:** Your full Gmail address (e.g., `you@gmail.com`)
    - **Password:** Your Google account password (or an App Password if you use 2FA - see note below).
    - **Server Address:** `https://www.google.com`
6. Click **Sign In**. This bypasses the buggy web view and uses a direct server connection.

**Important Note on 2-Factor Authentication (2FA):** If you have 2FA enabled on your Google account, you cannot use your regular password for CalDAV. You must generate an **App Password**:
1. Go to your Google Account security settings: [https://myaccount.google.com/security](https://myaccount.google.com/security)
2. Under "How you sign in to Google," find **2-Step Verification** and make sure it's on.
3. Then, search for "App passwords" or find it in the 2SV settings.
4. Generate a password for the app "Other (Custom name)" and name it "macOS Calendar".
5. Use this 16-digit generated password in the Calendar app instead of your regular Google password.

