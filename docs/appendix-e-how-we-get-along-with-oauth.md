
# How we get along with OAuth

### The Issue with OAuth

Duplicati is a backup tool running in the background. This usually means that there is no way to prompt users for input easily. This gets even more important when the command line interface is used to run backup scripts automatically and unattended.

Unfortunately, OneDrive and other services are designed to work with OAuth. OAuth is designed to grant access for a limited amount of time after users have authenticated correctly. This is not well suited for background services such as Duplicati which require to get access permissions regularly without the users being prompted to login.

To remedy this, we provide a service for Duplicati 2.0 users. The service handles the requirements for the OAuth login, and then uses this service to grant Duplicati access to the stored data. This service does neither store your OneDrive username nor your password. It is a bit difficult to explain how the service is working in details. But let's try!

With OAuth, the users start on a Duplicati page to set up a new connection. There users find a link which redirects to the OneDrive login page. After the users log in, OneDrive displays the desired access and the application name. The users accept this and they are redirected back to the Duplicati page. When this happens, an exchange is performed in the background where Duplicati and OneDrive exchange tokens. The token grants Duplicati access for a limited amount of time.

Furthermore, OneDrive allows users to block an application entirely. That is why OneDrive needs an "app secret" that identifies the application. This secret cannot be shared with anyone, so it stays entirely in the Duplicati service. Leaking this "app secret" would allow an attacker to impersonate Duplicati. This means, putting the app secret into the Duplicati application is a no-go.

When Duplicati sends the "app secret" to OneDrive, it responds with a unique numeric Windows Live user id (CID), and a "refresh token". Duplicati then generates a random authid token, which is used to encrypt the refresh token and CID inside the service. When a backup starts, Duplicati sends the authid token to the service, which decrypts the refresh token in-memory. The refresh token is then sent to OneDrive together with the "app secret". OneDrive grants an "access token" which is valid for one hour. This access token is returned to the Duplicati instance and used to upload and download files. If the backup takes longer than an hour, the process is repeated automatically.

This slightly complicated multi-step approach protects your Windows Live login details, so you never share your actual username (email) nor your password with Duplicati.

On top of this, the service encrypts all information completely so it cannot be used without the authid token. This ensures that an attacker needs to listen in on a live session to obtain the authid and access token. A full exposure of the service database is no risk to your data alone. You can create as many authid tokens as you like, and you can always revoke an authid token on the service if you fear it has been compromised.

### How it works in nine steps

1. To configure a OneDrive connection the user follows a link in the UI that requests access to OneDrive from "the service".
2. To grant access to OneDrive user has to confirm by logging in at OneDrive.
3. OneDrive then connects to the service and provides a user CID and a refresh-token.
4. The service stores the CID as well as the refresh-token encrypted. The service also generates an authid token for that user. The authid token is presented to the user and manually copied into Duplicati. The connection has been set up now.
5. When Duplicati wants to establish a connection, it sends the authid token to the service . the service uses the provided password to decrypt the refresh-token.
6. The refresh-token is sent to OneDrive along with the app secret.
7. OneDrive checks the refresh-token and gives back a one-hour access token that grants access to the real user account.
8. The service hands back the one-hour-access-token to Duplicati.
9. Duplicati uses the one-hour-access-token to connect to OneDrive. As soon as the connection fails, it requests a new token the same way the first one-hour-access-token was requested.

### What if the service gets hacked?

Then the hackers get a lot of blobs with encrypted refresh-tokens. Without the users' authid, the refresh-tokens are useless. Without the service's app secret, the authid's are useless. With one of them being useless, the other part is useless as well. In summary, if the service gets hacked, the hackers get a lot of useless stuff.

