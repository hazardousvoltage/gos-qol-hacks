# gos-qol-hacks

Small GrapheneOS QoL hacks I'm maintaining locally, most of which seem pretty unlikely to be upstreamed, and probably shouldn't be.  GOS maintainers welcome to take anything they like.



*LSCOPES*: A somewhat credible implementation of the GOS vision of Location Scopes.  Allows selection of Location Scopes on the AppInfo page or on the initial location perm request.  This implementation differs from the usual GOS Scopes behavior in that the grants the permission and fakes the location data in SystemServer, rather than denying the permission and faking the grant and the data in the app context.  I think this approach might give better coverage of the passive location paths.  Or maybe it was just easier and feels more natural for an old systems programmer.  You can choose a fixed location or noisy radius around the actual location, on a per-app basis.  Noisy mode caches the result to prevent apps tracking movements or averaging out positions, with a configurable TTL.  Setting large or small values for TTL and radius can also give you other effects, like "really random on every request" or "capture current location and save it for a long time."



*CLIPBOARD*: A minimalist AppInfo page toggle to set OP_READ_CLIPBOARD=ignored on a per-app basis, and show a toast if apps try to read anyway.  There's a little edge-case coverage for same-uid readbacks.  This app-op setting will in most cases prevent apps even attempting to read the clipboard, it will hide the "paste" option on context menus, etc.  If they try it anyway, you'll see a toast.



*DATASWITCH*: The "switch mobile networks" QS tile, pretty much directly pinched from crDroid and pasted in here.  Toggles the active data SIM between two available.  Only visible on devices with two data SIMs available.  Not included in QS shade by default.



*VPNTETHER*: Also pinched from crDroid.  Force tethering clients to use upstream VPN.  Puts a toggle on Tethering settings page.  If it's on, tethering clients will use VPN if connected.  If VPN isn't present or is lost, nulls out the upstream, which should be a fail-closed condition.  I hope.



*DURESS*: This is highly WIP and may not be very good.  Lets you designate a fingerprint as "duress", and presenting that print at keyguard triggers an immediate unconfirmed reboot, landing in BFU state.  At least under USA law, law enforcement can compel presentation of a biometric but cannot compel divulging a password or PIN.  This lets users have biometric lock with at least a veneer of safety: you can use a weird finger for your normal biometric unlock, and set the "expected" one as duress.  Your attacker has at best a 1-in-10 chance of effecting a successful unlock, unless they surveil you to see which finger you normally use.  I chose to reboot rather than either trigger lockdown or trigger a data wipe, because it seems the most "duress-shaped" behavior while guarding against inadvertently presenting the wrong print, or the hardware getting it wrong, and wiping your device.  This is much less secure than just not using biometrics, and you should still trigger "lockdown mode" when going into shady situations.  You have been warned.  
