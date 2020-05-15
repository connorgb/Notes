## 1: Security in the World of Web Applications
### Information Security in a Nutshell
#### Flirting with Formal Solutions
#### Enter Risk Management
#### Enlightenment Through Taxonomy
#### Toward Practical Approaches
### A Brief History of the Web
#### Tales of the Stone Age: 1945 to 1994
#### The First Browser Wars: 1995 to 1999
#### The Boring Period: 2000 to 2003
#### Web 2.0 and the Second Browser Wars: 2004 and Beyond
### The Evolution of a Threat
#### The User as a Security Flaw
#### The Cloud, or the Joys of Communal Living
#### Nonconvergence of Visions 
#### Cross-Browser Interactions: Synergy in Failure
#### The Breakdown of the Client-Server Divide
# Anatomy of the Web
## 2: It Starts with a URL
### Uniform Resource Locator
#### Scheme Name
#### Indicator of a Hierarchical URL
#### Credentials to Access the Resource
#### Server Start
#### Server Port
#### Hierarchical File Path
#### Query String
#### Fragment ID
#### Putting It All Together Again
### Reserved Characters and Percent Encoding
#### Handling of Non-US-ASCII Text
### Common URL Schemes and Their Function
#### Browser-Supported, Document-Fetching Protocols
#### Protocols Claimed by Third-Party Applications and Plug-ins
#### Nonencapsulating Pseudo-Protocols
#### Ensapsulating Pseudo-Protocols
#### Closing Note on Scheme Detection
### Resolution of Relative URLs
### Security Engineering Cheat Sheet
#### When Constructing Brand-New URLs Based on User Input
#### When Designing URL Input Filters
#### When Decooding Parameters Recieved Through URLs
## 3: Hypertext Transfer Protocol
### Basic Syntax of HTTP Traffic
#### The Consequences of Supporting HTTP/0.9
#### Newline Handling Quirks
#### Proxy Requests
#### Resolution of Duplicate or Conflicting Headers
#### Semicolon-Delimited Header Values
#### Header Character Set and Encoding Schemes
#### Referer Header Behavior
### HTTP Request Types
#### GET
#### POST
#### HEAD
#### OPTIONS
#### PUT
#### DELETE
#### TRACE
#### CONNECT
#### Other HTTP Methods
### Server Response Codes
#### 200-299: Success
#### 300-399: Redirection and Other Status Messages
#### 400-499: Client-Side Error
#### 500-599: Server-Side Error
#### Consistency of HTTP Code Signaling
### Keepalive Sessions
### Chunked Data Transfers
### Caching Behavior
### HTTP Cookie Semantics
### HTTP Authentication
### Protocol-Level Encryption and Client Certificates
#### Extended Validation Certificates
#### Error-Handling Rules
### Security Engineering Cheat Sheet
#### When Handling User-Controlled Filenames in Content-Disposition Headers
#### When Putting User Data in HTTP Cookies
#### When Sending User-Controlled Location Headers
#### When Sending User-Controlled Redirect Headers
#### When Constructing Other Types of User-Controlled Requests or Responses
## 4: Hypertext Markup Language 
### Basic Concepts Behind HTML Documents
#### Document Parsing Modes
#### The Battle Over Semantics 
### Understanding HTML Parser Behavior
#### Interactions Between Multiple Tags
#### Explicit and Implicit Conditionals
#### HTML Parsing Survival Tips 
### Entity Encoding
### HTTP/HTML Integration Semantics 
### Hyperlinking and Content Inclusion
#### Plain Links
#### Forms and Form-Triggered Requests
#### Frames
#### Type-Specific Content Inclusion
#### A Note on Cross-Site Request Forgery
### Security Engineering Cheat Sheet
#### Good Engineering Hygiene for All HTML Documents
#### When Generating HTML Documents with Attacker-Controlled Bits
#### When Converting HTML to Plaintext
#### When Writing a Markup Filter for User Content
## 5: Cascading Style Sheets
### Basic CSS Syntax
#### Property Definitions
#### @ Directives and XBL Bindings
#### Interactions with HTML
### Parser Resynchronization Risks
### Character Encoding
### Security Engineering Cheat Sheet
#### When Loading Remote Stylesheets
#### When Putting Attacker-Controlled Values into CSS
#### When Filtering User-Supplied CSS
#### When Allowing User-Specified Class Values on HTML Markup
## 6: Browser-Side Scripts
### Basic Characteristics of Javascript
#### Script Processing Model
#### Execution Ordering Control
#### Code and Object Inspection Capabilities
#### Modifying the Runtime Environment
#### Javascript Object Notation and Other Data Serializations
#### E4X and Other Syntax Extensions
### Standard Object Hierarchy 
#### The Document Object Model
#### Access to Other Documents
### Script Character Encoding
### Code Inclusion Modes and Nesting Risks
### The Living Dead: Visual Basic
### Security Engineering Cheat Sheet
#### When Loading Remote Scripts
#### When Parsing JSON Recieved from the Server
#### When Putting User-Supplied Data Inside Javascript Blocks
#### When Interacting with Browser Objects on the Client Side
#### If You Want to Allow User-Controlled Scripts on Your Page
## 7: Non-HTML Document Types
### Plaintext Files
### Bitmap Images
### Audio and Video
### XML-Based Documents
#### Generic XML View
#### Scalable Vector Graphics
#### Mathematical Markup Language
#### XML User Interface Language
#### Wireless Markup Language
#### RSS and Atom Feeds
### A Note on Nonrenderable File Types
### Security Engineering Cheat Sheet
#### When Hosting XML-Based Document Formats
#### On All Non-HTML Document Types
## 8: Content Rendering With Browser Plug-ins
### Invoking a Plug-in
#### The Perils of Content-Type Handling
### Document Rendering Helpers
### Plug-in-Based Application Frameworks
#### Adobe Flash
#### Microsoft Silverlight
#### Sun Java
#### XML Browser Applications (XBAP)
### ActiveX Protocols
### Living with Other Plug-ins
### Security Engineering Cheat Sheet
#### When Serving Plug-in-Handled Files
#### When Embedding Plug-in-Handled Files
#### If You Want to Write a New Browser Plug-in or ActiveX Component
# Browser Security Features
## 9: Content Isolation Logic
### Same-Origin Policy for the Document Object Model
#### document.domain
#### postMessage(...)
#### Interactions with Browser Credentials
### Same-Origin Policy for XMLHttpRequest
### Same-Origin Policy for Web Storage
### Security Policy for Cookies
#### Impact of Cookies on the Same-Origin Policy
#### Problems with Domain Restrictions
#### The Unusual Danger of "localhost"
#### Cookies and "Legitimate" DNS Hijacking
### Plug-in Security Rules
#### Adobe Flash
#### Microsoft Silverlight
#### Java
### Coping with Ambiguous or Unexpected Origins
#### IP Addresses
#### Hostnames with Extra Periods
#### Non-Fully Qualified HostNames
#### Local Files 
#### Pseudo-URLs
#### Browser Extensions and UI
### Other Uses of Origins
### Security Engineering Cheat Sheet
#### Good Security Policy Hygiene for All Websites
#### When Relying on HTTP Cookies for Authentication
#### When Arranging Cross-Domain Communications in JavaScript
#### When Embedding Plug-in-Handled Active Content from Third Parties
#### When Hosting Your Own Plug-in-Executed Content
#### When Writing Browser Extensions
## 10: Origin Inheritance
### Origin Inheritance for about:blank
### Inheritance for data: URLs
### Inheritance for javascript: and vbscript: URLs
### A Note on Restricted Pseudo-URLs
### Security Engineering Cheat Sheet
## 11: Life Outside Same-Origin Rules
### Window and Frame Interactions
#### Changing the Location of Existing Documents
#### Unsolicited Framing
### Cross-Domain Content Inclusion
#### A Note on Cross-Origin Subresources
### Privacy-Related Side Channels
### Other SOP Loopholes and Their Uses
### Security Engineering Cheat Sheet
#### Good Security Engineering for All Websites
#### When Including Cross-Domain Resources
#### When Arranging Cross-Domain Communications in JavaScript
## 12: Other Security Boundaries
### Navigation to Sensitive Schemes
### Access to Internal Networks
### Prohibited Ports
### Limitations on Third-Party Cookies
### Security Engineering Cheat Sheet
#### When Building Web Applications on Internal Networks
#### When Launching Non-HTTP Services, Particularly on Nonstandard Ports
#### When Using Third-Party Cookies for Gadgets or Sandboxed Content
## 13: Content Recognition Mechanisms
### Document Type Detection Logic
#### Malformed MIME Types 
#### Special Content-Type Values
#### Unrecognized Content Type
#### Defensive Uses of Content-Disposition
#### Content Directives on Subresources
#### Downloaded Files and Other Non-HTTP Content
### Character Set Handling
#### Byte Order Marks
#### Character Set Inheritance and Override
#### Markup-Controlled Charset on Subresources
#### Detection for Non-HTTP Files
### Security Engineering Cheat Sheet
#### Good Security Practices for All Websites
#### When Generating Documents with Partly Attacker-Controlled Contents
#### When Hosting User-Generated Files
## 14: Dealing with Rogue Scripts
### Denial-of-Service Attacks
#### Execution Time and Memory Use Restrictions
#### Connection Limits
#### Pop-Up Filtering
#### Dialog Use Restrictions
### Window-Positioning and Appearance Problems
### Timing Attacks on User Interfaces
### Security Engineering Cheat Sheet
#### When Permitting User-Created `<iframe>` Gadgets on Your Site
#### When Building Security-Sensitive UIs
## 15: Extrinsic Site Privileges
### Browser- and Plug-in-Managed Site Permissions
#### Hardcoded Domains
### Form-Based Password Managers
### Internet Explorer's Zone Model
#### Mark of the Web and Zone.Identifier
### Security Engineering Cheat Sheet
#### When Requesting Elevated Permissions from Within a Web Application
#### When Writing Plug-ins or Extensions That Recognize Privileged Origins
# A Glimpse of Things to Come
## 16: New and Upcoming Security Features
### Security Model Restriction Frameworks
#### Cross-Domain Requests
#### XDomainRequest
#### Other Uses of the Origin Header
### Security Model Restriction Frameworks
#### Content Security Policy
#### Sandboxed Frames
#### Strict Transport Security
#### Private Browsing Modes
### Other Developments
#### In-Browser HTML Sanitizers
#### XSS Filtering
### Security Engineering Cheat Sheet
## 17: Other Browser Mechanisms of Note
### URL-and Protocol-Level Proposals
### Content-Level Features
### I/O Interfaces
## 18: Common Web Vulnerabilities
### Vulnerabilities Specific to Web Applications
### Problems to Keep in Mind in Web Application Design
### Common Problems Unique to Server-Side Code