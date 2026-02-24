# Copilot Retrieval API — Quick Start

The Copilot Retrieval API requires a **delegated** (user) token.
This uses the **device code flow** — no passwords stored, works with MFA.

---

## Step 1: Register an App (one-time, ~5 min)

1. Open [portal.azure.com](https://portal.azure.com) → search **"App registrations"** → click it
2. Click **+ New registration**
   - **Name:** `Retrieval API Test`
   - **Supported account types:** leave the default (single tenant)
3. Click **Register**
4. Copy the **Application (client) ID** and **Directory (tenant) ID**
5. In the left sidebar, click **Authentication** → open **Settings** tab → set **Allow public client flows** to **Yes** → click **Save**
6. In the left sidebar, click **API permissions** → **+ Add a permission** → **Microsoft Graph** → **Delegated permissions**
7. Search for `Sites.Read.All`, check it, click **Add permissions**
8. Click **Grant admin consent**

---

## Step 2: Run the `.http` file

1. Install the **REST Client** extension in VS Code (search "REST Client" by Humao)
2. Open `retrieval-api.http`
3. Fill in `@tenantId` and `@clientId` with the values from Step 1
4. Click **Send Request** on each step (1 → 2 → 3)
   - Step 1: starts login → go to browser, enter the code, sign in
   - Step 2: gets the token (automatic, no pasting)
   - Step 3: calls the API (automatic, no pasting)

---

## What You Should See

```json
{
    "retrievalHits": [
        {
            "webUrl": "https://contoso.sharepoint.com/.../Q4Budget.docx",
            "extracts": [{
                "text": "FY26 Q4 budget allocation...",
                "relevanceScore": 0.94
            }],
            "resourceType": "listItem",
            "resourceMetadata": {
                "title": "Q4 Budget Plan",
                "author": "Jane Smith"
            },
            "sensitivityLabel": { }
        }
    ]
}
```

---

## Something not working?

| You see | What's wrong | Fix |
|---------|-------------|-----|
| `401 Unauthorized` | Token expired (they last ~1 hour) | Re-run Step 1 in the .http file to get a fresh token |
| `403 Forbidden` | Missing permissions | Make sure `Sites.Read.All` (Delegated) is granted with admin consent |
| `404 Not Found` | API not available | Your tenant may not have Copilot licenses |
| `400 Bad Request` | Typo in the JSON body | Compare your request to the examples above |
