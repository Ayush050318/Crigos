<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>My Account — CRIGO</title>
  <meta name="description" content="Manage your CRIGO account — view orders, update your details, and track deliveries."/>
  <link rel="preconnect" href="https://fonts.googleapis.com"/>
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet"/>
  <link rel="stylesheet" href="style.css"/>
  <link rel="stylesheet" href="account.css"/>
  <!-- Supabase SDK (CDN) -->
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/dist/umd/supabase.min.js"></script>
</head>
<body>

  <!-- ANNOUNCEMENT -->
  <div class="announcement">A little lighter. A lot more you. <span>✦</span> Meet the CRIGO launch collection</div>

  <!-- HEADER -->
  <header class="site-header">
    <div class="flex items-center gap-2">
      <button class="icon-btn mobile-menu-button" onclick="toggleMobileMenu()">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 5h16"/><path d="M4 12h16"/><path d="M4 19h16"/></svg>
      </button>
      <a href="index.html" class="brand-link"><span class="brand-text">CRIGO</span></a>
    </div>
    <nav class="desktop-nav">
      <a href="index.html">Shop all</a>
      <a href="index.html">Everyday essentials</a>
      <a href="index.html">The style edit</a>
      <a href="index.html">Charm studio <span class="text-coral">✦</span></a>
      <a href="index.html">Our story</a>
    </nav>
    <div class="header-tools">
      <button class="icon-btn" onclick="window.location.href='index.html#products'">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m21 21-4.34-4.34"/><circle cx="11" cy="11" r="8"/></svg>
      </button>
      <button class="icon-btn cart-btn" onclick="window.location.href='index.html'">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 10a4 4 0 0 1-8 0"/><path d="M3.103 6.034h17.794"/><path d="M3.4 5.467a2 2 0 0 0-.4 1.2V20a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6.667a2 2 0 0 0-.4-1.2l-2-2.667A2 2 0 0 0 17 2H7a2 2 0 0 0-1.6.8z"/></svg>
      </button>
      <button class="icon-btn login-btn" onclick="signOut()" title="Sign Out">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
      </button>
    </div>
  </header>

  <!-- MOBILE NAV -->
  <div class="mobile-nav" id="mobile-nav">
    <a href="index.html">Shop all</a>
    <a href="index.html">Everyday essentials</a>
    <a href="index.html">The style edit</a>
    <a href="index.html">Charm studio ✦</a>
    <a href="index.html">Our story</a>
  </div>

  <!-- LOADING SCREEN -->
  <div id="page-loading" class="page-loading">
    <div class="loading-spinner"></div>
    <p>Loading your account…</p>
  </div>

  <!-- NOT LOGGED IN STATE -->
  <div id="not-logged-in" class="not-logged-in" style="display:none">
    <div class="nli-content">
      <div class="nli-icon">
        <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
      </div>
      <h2>Sign in to your account</h2>
      <p>View your orders, manage your details, and track deliveries — all in one place.</p>
      <a href="index.html" class="btn btn-primary">Sign In / Sign Up</a>
    </div>
  </div>

  <!-- MAIN ACCOUNT PAGE (shown when logged in) -->
  <div id="account-page" class="account-page" style="display:none">

    <!-- ACCOUNT HEADER -->
    <div class="account-hero">
      <div class="container-wide">
        <div class="account-hero-inner">
          <div class="account-avatar-large" id="account-avatar-large">A</div>
          <div class="account-hero-info">
            <div class="eyebrow">My Account</div>
            <h1 id="account-hero-name">Loading…</h1>
            <p id="account-hero-email" class="account-hero-sub"></p>
          </div>
          <div class="account-hero-stats">
            <div class="stat-box">
              <span class="stat-num" id="stat-orders">–</span>
              <span class="stat-label">Orders</span>
            </div>
            <div class="stat-box">
              <span class="stat-num" id="stat-delivered">–</span>
              <span class="stat-label">Delivered</span>
            </div>
            <div class="stat-box">
              <span class="stat-num" id="stat-spent">–</span>
              <span class="stat-label">Total Spent</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- SIDEBAR + CONTENT -->
    <div class="account-layout container-wide">

      <!-- SIDEBAR NAV -->
      <aside class="account-sidebar">
        <nav class="sidebar-nav">
          <button class="sidebar-link active" id="nav-profile" onclick="showSection('profile')">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
            Profile
          </button>
          <button class="sidebar-link" id="nav-orders" onclick="showSection('orders')">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 10a4 4 0 0 1-8 0"/><path d="M3.103 6.034h17.794"/><path d="M3.4 5.467a2 2 0 0 0-.4 1.2V20a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6.667a2 2 0 0 0-.4-1.2l-2-2.667A2 2 0 0 0 17 2H7a2 2 0 0 0-1.6.8z"/></svg>
            My Orders
          </button>
          <button class="sidebar-link" id="nav-addresses" onclick="showSection('addresses')">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/></svg>
            Saved Addresses
          </button>
          <button class="sidebar-link" id="nav-security" onclick="showSection('security')">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
            Security
          </button>
          <button class="sidebar-link sidebar-signout" onclick="signOut()">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/></svg>
            Sign Out
          </button>
        </nav>
      </aside>

      <!-- MAIN CONTENT -->
      <div class="account-content">

        <!-- ===== PROFILE SECTION ===== -->
        <section class="acct-section" id="section-profile">
          <div class="section-card">
            <div class="section-card-header">
              <h2>Personal Details</h2>
              <button class="btn-ghost" id="edit-profile-btn" onclick="toggleEditProfile()">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"/></svg>
                Edit
              </button>
            </div>

            <!-- VIEW MODE -->
            <div id="profile-view">
              <div class="details-grid">
                <div class="detail-item">
                  <span class="detail-label">Full Name</span>
                  <span class="detail-value" id="view-name">—</span>
                </div>
                <div class="detail-item">
                  <span class="detail-label">Mobile Number</span>
                  <span class="detail-value" id="view-mobile">—</span>
                </div>
                <div class="detail-item">
                  <span class="detail-label">Email Address</span>
                  <span class="detail-value" id="view-email">—</span>
                </div>
                <div class="detail-item">
                  <span class="detail-label">Member Since</span>
                  <span class="detail-value" id="view-since">—</span>
                </div>
              </div>
            </div>

            <!-- EDIT MODE -->
            <form id="profile-edit" style="display:none" onsubmit="saveProfile(event)">
              <div class="form-row">
                <div class="form-group">
                  <label for="edit-name">Full Name</label>
                  <input type="text" id="edit-name" required/>
                </div>
                <div class="form-group">
                  <label for="edit-mobile">Mobile Number</label>
                  <div class="input-with-icon">
                    <span class="input-prefix">🇮🇳 +91</span>
                    <input type="tel" id="edit-mobile" maxlength="10"/>
                  </div>
                </div>
              </div>
              <div class="form-group">
                <label for="edit-email">Email Address</label>
                <input type="email" id="edit-email"/>
                <small class="field-note">Changing your email will require re-verification.</small>
              </div>
              <div class="form-actions">
                <button type="button" class="btn btn-outline" onclick="toggleEditProfile()">Cancel</button>
                <button type="submit" class="btn btn-primary">Save Changes</button>
              </div>
            </form>
          </div>
        </section>

        <!-- ===== ORDERS SECTION ===== -->
        <section class="acct-section" id="section-orders" style="display:none">
          <div class="section-card">
            <div class="section-card-header">
              <h2>My Orders</h2>
              <div class="order-filter">
                <select id="order-filter-sel" onchange="filterOrders(this.value)">
                  <option value="all">All Orders</option>
                  <option value="confirmed">Confirmed</option>
                  <option value="processing">Processing</option>
                  <option value="shipped">Shipped</option>
                  <option value="delivered">Delivered</option>
                  <option value="cancelled">Cancelled</option>
                </select>
              </div>
            </div>
            <div id="orders-list">
              <div class="orders-loading">
                <div class="loading-spinner small"></div>
                <span>Loading orders…</span>
              </div>
            </div>
          </div>
        </section>

        <!-- ===== ADDRESSES SECTION ===== -->
        <section class="acct-section" id="section-addresses" style="display:none">
          <div class="section-card">
            <div class="section-card-header">
              <h2>Saved Addresses</h2>
              <button class="btn btn-outline btn-sm" onclick="openAddressForm()">+ Add New</button>
            </div>
            <div id="addresses-list">
              <div class="orders-loading">
                <div class="loading-spinner small"></div>
              </div>
            </div>

            <!-- Address form (hidden by default) -->
            <div id="address-form-wrap" style="display:none">
              <div class="divider"></div>
              <h3 class="form-section-title">New Address</h3>
              <form id="address-form" onsubmit="saveAddress(event)" class="auth-form">
                <div class="form-row">
                  <div class="form-group">
                    <label for="adr-fname">First Name</label>
                    <input type="text" id="adr-fname" required/>
                  </div>
                  <div class="form-group">
                    <label for="adr-lname">Last Name</label>
                    <input type="text" id="adr-lname" required/>
                  </div>
                </div>
                <div class="form-group">
                  <label for="adr-phone">Phone</label>
                  <div class="input-with-icon">
                    <span class="input-prefix">🇮🇳 +91</span>
                    <input type="tel" id="adr-phone" maxlength="10" required/>
                  </div>
                </div>
                <div class="form-group">
                  <label for="adr-addr1">Address Line 1</label>
                  <input type="text" id="adr-addr1" required/>
                </div>
                <div class="form-group">
                  <label for="adr-addr2">Address Line 2 <span class="optional">(optional)</span></label>
                  <input type="text" id="adr-addr2"/>
                </div>
                <div class="form-row">
                  <div class="form-group">
                    <label for="adr-city">City</label>
                    <input type="text" id="adr-city" required/>
                  </div>
                  <div class="form-group">
                    <label for="adr-pincode">Pincode</label>
                    <input type="text" id="adr-pincode" maxlength="6" required/>
                  </div>
                </div>
                <div class="form-group">
                  <label for="adr-state">State</label>
                  <select id="adr-state" required>
                    <option value="">Select State</option>
                    <option>Andhra Pradesh</option><option>Arunachal Pradesh</option>
                    <option>Assam</option><option>Bihar</option><option>Chhattisgarh</option>
                    <option>Goa</option><option>Gujarat</option><option>Haryana</option>
                    <option>Himachal Pradesh</option><option>Jharkhand</option>
                    <option>Karnataka</option><option>Kerala</option><option>Madhya Pradesh</option>
                    <option>Maharashtra</option><option>Manipur</option><option>Meghalaya</option>
                    <option>Mizoram</option><option>Nagaland</option><option>Odisha</option>
                    <option>Punjab</option><option>Rajasthan</option><option>Sikkim</option>
                    <option>Tamil Nadu</option><option>Telangana</option><option>Tripura</option>
                    <option>Uttar Pradesh</option><option>Uttarakhand</option><option>West Bengal</option>
                    <option>Delhi</option><option>Jammu & Kashmir</option><option>Ladakh</option>
                    <option>Puducherry</option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="adr-label">Label</label>
                  <div class="label-pills">
                    <button type="button" class="label-pill active" onclick="setLabel(this,'Home')">🏠 Home</button>
                    <button type="button" class="label-pill" onclick="setLabel(this,'Work')">💼 Work</button>
                    <button type="button" class="label-pill" onclick="setLabel(this,'Other')">📍 Other</button>
                  </div>
                  <input type="hidden" id="adr-label" value="Home"/>
                </div>
                <div class="form-actions">
                  <button type="button" class="btn btn-outline" onclick="closeAddressForm()">Cancel</button>
                  <button type="submit" class="btn btn-primary">Save Address</button>
                </div>
              </form>
            </div>
          </div>
        </section>

        <!-- ===== SECURITY SECTION ===== -->
        <section class="acct-section" id="section-security" style="display:none">
          <div class="section-card">
            <div class="section-card-header">
              <h2>Security</h2>
            </div>
            <div class="security-items">
              <div class="security-item">
                <div class="security-item-info">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
                  <div>
                    <strong>Password</strong>
                    <p>Last changed: Unknown</p>
                  </div>
                </div>
                <button class="btn btn-outline btn-sm" onclick="sendPasswordReset()">Reset via Email</button>
              </div>
              <div class="security-item">
                <div class="security-item-info">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07A19.5 19.5 0 0 1 4.69 12 19.79 19.79 0 0 1 1.61 3.42 2 2 0 0 1 3.58 1h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L7.91 8.55a16 16 0 0 0 5.49 5.49l.92-.92a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/></svg>
                  <div>
                    <strong>Mobile Number</strong>
                    <p id="security-mobile">Not set</p>
                  </div>
                </div>
                <button class="btn btn-outline btn-sm" onclick="showSection('profile'); toggleEditProfile()">Update</button>
              </div>
              <div class="security-item danger-zone">
                <div class="security-item-info">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
                  <div>
                    <strong>Delete Account</strong>
                    <p>Permanently delete your account and all data</p>
                  </div>
                </div>
                <button class="btn btn-danger btn-sm" onclick="confirmDeleteAccount()">Delete</button>
              </div>
            </div>
          </div>
        </section>

      </div><!-- /.account-content -->
    </div><!-- /.account-layout -->
  </div><!-- /#account-page -->

  <!-- ORDER DETAIL MODAL -->
  <div class="modal-overlay" id="order-detail-modal">
    <div class="modal order-detail-inner">
      <button class="modal-close" onclick="closeOrderDetail()">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
      </button>
      <div id="order-detail-content"></div>
    </div>
  </div>

  <!-- TOAST -->
  <div class="toast" id="toast"></div>

  <script src="account.js"></script>
</body>
</html>
