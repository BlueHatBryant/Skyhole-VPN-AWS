Commonly Whitelisted Domains
Whitelisting Tips
List any tips you've discovered on how or what to whitelist in order to solve specific issues!

Google (Maps, Youtube, etc)
Google Maps and other Google services
pihole -w clients4.google.com 
pihole -w clients2.google.com
YouTube history
pihole -w s.youtube.com 
pihole -w video-stats.l.google.com
YouTube App for iOS
pihole -w www.googleapis.com 
pihole -w youtubei.googleapis.com
pihole -w oauthaccountmanager.googleapis.com
Google Play
As described here.

pihole -w android.clients.google.com
Google Keep
The Google Keep Chrome App requires these domains to be whitelisted in order to sync.

pihole -w reminders-pa.googleapis.com firestore.googleapis.com
Google Fonts
pihole -w gstaticadssl.l.google.com
Gmail (Google Mail)
iOS app requires this to be whitelisted for the app to connect.

pihole -w googleapis.l.google.com
Google Chrome (to update on ubuntu)
pihole -w dl.google.com
Android TV
pihole -w redirector.gvt1.com
Microsoft (Windows, Office, Skype, etc)
Windows uses this to verify connectivity to Internet
pihole -w www.msftncsi.com www.msftconnecttest.com
Microsoft Web Pages (Outlook, Office365, Live, Microsoft.com...)
pihole -w outlook.office365.com products.office.com c.s-microsoft.com i.s-microsoft.com login.live.com login.microsoftonline.com 
Backup bitlocker recovery key to Microsoft account
pihole -w g.live.com
Microsoft Store (Windows Store)
pihole -w dl.delivery.mp.microsoft.com geo-prod.do.dsp.mp.microsoft.com displaycatalog.mp.microsoft.com
Windows 10 Update
pihole -w sls.update.microsoft.com.akadns.net fe3.delivery.dsp.mp.microsoft.com.nsatc.net tlu.dl.delivery.mp.microsoft.com
Microsoft Edge Browser Update
pihole -w msedge.api.cdp.microsoft.com
Xbox Live
This domain is used for sign-ins, creating new accounts, and recovering existing Microsoft accounts on your (confirmed by Microsoft)

pihole -w clientconfig.passport.net 
Xbox Live Achievements (confirmed by Microsoft)
pihole -w v10.events.data.microsoft.com
pihole -w v20.events.data.microsoft.com
Xbox Live Messaging (post)
pihole -w client-s.gateway.messenger.live.com
Store App on Series X/S
pihole -w arc.msn.com
EA Play on Xbox
pihole -w activity.windows.com
Full Functionality
There are several domains discovered initially on Reddit and /r/xboxone, which were also confirmed by Microsoft as being required by Xbox Live for full functionality.

pihole -w xbox.ipv6.microsoft.com device.auth.xboxlive.com www.msftncsi.com title.mgt.xboxlive.com xsts.auth.xboxlive.com title.auth.xboxlive.com ctldl.windowsupdate.com attestation.xboxlive.com xboxexperiencesprod.experimentation.xboxlive.com xflight.xboxlive.com cert.mgt.xboxlive.com xkms.xboxlive.com def-vef.xboxlive.com notify.xboxlive.com help.ui.xboxlive.com licensing.xboxlive.com eds.xboxlive.com www.xboxlive.com v10.vortex-win.data.microsoft.com settings-win.data.microsoft.com
Skype
See the GitHub Topic on these domains.

pihole -w s.gateway.messenger.live.com client-s.gateway.messenger.live.com ui.skype.com pricelist.skype.com apps.skype.com m.hotmail.com sa.symcb.com s{1..5}.symcb.com 
Microsoft Office
Reddit link - r/pihole - MS Office issues

pihole -w officeclient.microsoft.com
Bing Maps Platform
as described here.

pihole -w dev.virtualearth.net ecn.dev.virtualearth.net t0.ssl.ak.dynamic.tiles.virtualearth.net t0.ssl.ak.tiles.virtualearth.net
Apple
Apple Music
pihole -w itunes.apple.com
pihole -w s.mzstatic.com
Apple ID
pihole -w appleid.apple.com
iOS Weather app
pihole -w gsp-ssl.ls.apple.com
pihole -w gsp-ssl.ls-apple.com.akadns.net
Captive-portal tests
These domains are checked by the operating systems when connecting via wifi, and if they don't get the response they expect, they may try to open a wifi login page or similar as they believe they are located behind a captive portal.

Android/Chrome
pihole -w connectivitycheck.android.com android.clients.google.com clients3.google.com connectivitycheck.gstatic.com 
Windows/Microsoft
pihole -w msftncsi.com www.msftncsi.com ipv6.msftncsi.com
iOS/Apple
(note that this does not cover all domains used older iOS versions)

pihole -w captive.apple.com gsp1.apple.com www.apple.com www.appleiphonecell.com
Other
Jackbox.tv
Jackbox.tv will not load unless you whitelist google-analytics. As of October 2020, this may no longer be necessary.

pihole -w www.google-analytics.com
pihole -w ssl.google-analytics.com
Spotify
The Spotify app for iOS will stop functioning unless it's web service counterpart is whitelisted.

pihole -w spclient.wg.spotify.com apresolve.spotify.com
For spotify to work on TVs the API might need whitelisted:

pihole -w api-tv.spotify.com
Target's Weekly Ads
pihole -w weeklyad.target.com m.weeklyad.target.com weeklyad.target.com.edgesuite.net
Facebook, Facebook Messenger
pihole -w upload.facebook.com creative.ak.fbcdn.net external-lhr0-1.xx.fbcdn.net external-lhr1-1.xx.fbcdn.net external-lhr10-1.xx.fbcdn.net external-lhr2-1.xx.fbcdn.net external-lhr3-1.xx.fbcdn.net external-lhr4-1.xx.fbcdn.net external-lhr5-1.xx.fbcdn.net external-lhr6-1.xx.fbcdn.net external-lhr7-1.xx.fbcdn.net external-lhr8-1.xx.fbcdn.net external-lhr9-1.xx.fbcdn.net fbcdn-creative-a.akamaihd.net scontent-lhr3-1.xx.fbcdn.net scontent.xx.fbcdn.net scontent.fgdl5-1.fna.fbcdn.net graph.facebook.com b-graph.facebook.com connect.facebook.com cdn.fbsbx.com api.facebook.com edge-mqtt.facebook.com mqtt.c10r.facebook.com portal.fb.com star.c10r.facebook.com star-mini.c10r.facebook.com b-api.facebook.com fb.me bigzipfiles.facebook.com l.facebook.com www.facebook.com scontent-atl3-1.xx.fbcdn.net static.xx.fbcdn.net edge-chat.messenger.com video.xx.fbcdn.net external-ort2-1.xx.fbcdn.net scontent-ort2-1.xx.fbcdn.net edge-chat.facebook.com scontent-mia3-1.xx.fbcdn.net web.facebook.com rupload.facebook.com l.messenger.com
DirectTV
Sourced from here.

pihole -w directvnow.com directvapplications.hb.omtrdc.net s.zkcdn.net js.maxmind.com
Bild DE
pihole -w www.asadcdn.com code.bildstatic.de de.ioam.de json.bild.de script.ioam.de tags.tiqcdn.com tagger.opecloud.com
Spiegel DE
pihole -w image.angebote.spiegel.de
Plex Domains
pihole -w plex.tv tvdb2.plex.tv pubsub.plex.bz proxy.plex.bz proxy02.pop.ord.plex.bz cpms.spop10.ams.plex.bz meta-db-worker02.pop.ric.plex.bz meta.plex.bz tvthemes.plexapp.com.cdn.cloudflare.net tvthemes.plexapp.com 106c06cd218b007d-b1e8a1331f68446599e96a4b46a050f5.ams.plex.services meta.plex.tv cpms35.spop10.ams.plex.bz proxy.plex.tv metrics.plex.tv pubsub.plex.tv status.plex.tv www.plex.tv node.plexapp.com nine.plugins.plexapp.com staging.plex.tv app.plex.tv o1.email.plex.tv  o2.sg0.plex.tv dashboard.plex.tv
Domains used by Plex
pihole -w gravatar.com # custom login pictures
pihole -w thetvdb.com # metadata for tv series
pihole -w themoviedb.com # metadata for movies
pihole -w chtbl.com # iHeart radio/Plex Podcast
Sonarr
pihole -w services.sonarr.tv skyhook.sonarr.tv download.sonarr.tv apt.sonarr.tv forums.sonarr.tv
Placehold.it (Image placeholders often used during web design. Not sure why this is even blocked in the first place.)
pihole -w placehold.it placeholdit.imgix.net
Dropbox
As described here:

pihole -w dl.dropboxusercontent.com ns1.dropbox.com ns2.dropbox.com
Fox News
as described here.

pihole -w widget-cdn.rpxnow.com
Images on Marketwatch.com
pihole -w s.marketwatch.com
GoDaddy webmail buttons
pihole -w imagesak.secureserver.net
WatchESPN
as described here.

pihole -w fpdownload.adobe.com entitlement.auth.adobe.com livepassdl.conviva.com
NVIDIA GeForce Experience
GFE requires this to download driver updates (or events.gfe.nvidia.com, but that is also used for telemetry).

pihole -w gfwsl.geforce.com
Videos not playing in times.com and nydailynews.com
pihole -w delivery.vidible.tv img.vidible.tv videos.vidible.tv edge.api.brightcove.com cdn.vidible.tv
Videos not playing on weather.com
pihole -w v.w-x.co
Moto phones OS updates
pihole -w appspot-preview.l.google.com
Grand Theft Auto V Online PC
Since March blocking Rockstar telemetry seems to crash GTAOnline.
Unblocking the domain fixed crashing for people at GTA Forums.

pihole -w prod.telemetry.ros.rockstargames.com
Chevrolet
Couldn't browse inventory w/o whitelisting

pihole -w chevrolet.com
Epic Games Store
The tracking domain is required to make a purchase on the website, otherwise the order modal will appear to be loading forever.
It is also required when logging into the Epic Launcher when using two-factor authentication.

pihole -w tracking.epicgames.com
Origin (Savegame-Sync)
pihole -w cloudsync-prod.s3.amazonaws.com
Red Hat Online Learning (subscription required)
embedded video player
When blocked, video would play; however, not show progress (continually showing it was loading) and thus not record your progression through the video in the course progress tracker.

pihole -w 79423.analytics.edgekey.net
Lowe's Checkout
Clicking "Start Secure Checkout" from the Shopping Cart on Lowe's website does not load anything.

pihole -w assets.adobedtm.com
Home Depot Checkout
Clicking "Checkout" from the Shopping Cart on Home Depot's website takes you to a page to check out as guest, login, or create account. Entering login credentials does not ungray the "Sign In" option. "Checkout as a Guest" and "Create an Account" options do not load anything.

pihole -w nexus.ensighten.com
Mozilla Firefox Tracking Protection
Firefox updates the black- and whitelists for Tracking Protection daily using a domain that might not only occur on some Blocklists, but also can be blocked as the result of regex blacklists for starting with "tracking". The domain should be whitelisted to ensure the Tracking Protection can be updated (when used at all), or it might render some websites useless on the Firefox Webbrowser in the long run.

pihole -w tracking-protection.cdn.mozilla.net
Playstation 5 "Recently Played Games" and Trophies
pihole -w telemetry-console.api.playstation.com
Cannon Printers
To be able to retrieve firmware updates

pihole -w gdlp01.c-wss.com
Reddit
pihole -w styles.redditmedia.com
pihole -w www.redditstatic.com
pihole -w reddit.map.fastly.net
pihole -w www.redditmedia.com
pihole -w reddit-uploaded-media.s3-accelerate.amazonaws.com
pihole --white-regex [a-z]\.thumbs\.redditmedia\.com
pihole --white-regex (\.|^)redd\.it$
pihole --white-regex (\.|^)reddit\.com$
Tracking Packages sent with DPD
pihole -w tracking.dpd.de
WhatsApp
pihole -w wa.me
pihole -w www.wa.me
pihole --white-regex ^whatsapp-cdn-shv-[0-9]{2}-[a-z]{3}[0-9]\.fbcdn\.net$
pihole --white-regex ^((www|(w[0-9]\.)?web|media((-[a-z]{3}|\.[a-z]{4})[0-9]{1,2}-[0-9](\.|-)(cdn|fna))?)\.)?whatsapp\.(com|net)$
Signal
pihole -w ud-chat.signal.org
pihole -w chat.signal.org
pihole -w storage.signal.org
pihole -w signal.org
pihole -w www.signal.org
pihole -w updates2.signal.org
pihole -w textsecure-service-whispersystems.org
pihole -w giphy-proxy-production.whispersystems.org
pihole -w cdn.signal.org
pihole -w whispersystems-textsecure-attachments.s3-accelerate.amazonaws.com
pihole -w d83eunklitikj.cloudfront.net
pihole -w souqcdn.com
pihole -w cms.souqcdn.com
pihole -w api.directory.signal.org
pihole -w contentproxy.signal.org
pihole -w turn1.whispersystems.org
Twitter
pihole -w twitter.com
pihole -w upload.twitter.com
pihole -w api.twitter.com
pihole -w mobile.twitter.com
pihole --white-regex (\.|^)twimg\.com$
Banks
TSB Mobile
pihole -w h-sdk.online-metrix.net

*Or Try:*

pihole -w check2.tsb.co.uk
Citizen's Bank
pihole -w p11.techlab-cdn.com
OLA MONEY
pihole -w logs.juspay.in
Resturants / Rewards
Burger King
pihole -w appboy-images.com rest.iad-03.braze.com
Punchh (Farmer Boys, El Pollo Loco, Capriotti's, etc.)
pihole -w mobileandroidapi.punchh.com
Rumble
The Rumble interface may have missing thumbnails and icons, and live streams present as a black screen. These are served from subdomains of rmbl.ws which should be added as a wildcard whitelist entry.

Dutch / The Netherlands websites
nu.nl (enable videos, tvgids et cetera)
pihole -w cds.s5x3j6q5.hwcdn.net
Swedish streaming services
svtplay (enable continuing where you left off)
pihole -w analytics.svt.se
