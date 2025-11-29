# Obsyde Upgrade Task List (v1.0)

## 🔐 Auth Flow Fixes
- [ ] Protect `/tasks` route on frontend for unauthenticated users
- [ ] Perform auth check before page renders to prevent flash
- [ ] Redirect to `/login` if not logged in
- [ ] Add a loading spinner or placeholder during auth check

## 🖼️ Profile Pic Handling
- [ ] Properly serve media files in production (`/media/` with DEBUG=False)
- [ ] Fix profile image URLs on frontend (use full backend URL)
- [ ] Add fallback profile image (optional)

## 📝 Task Display Improvements
- [ ] Show task `due_time` properly in UI
- [ ] Render task `description` neatly
- [ ] Optionally truncate long descriptions with a “Read more” toggle

## 📱 Responsiveness Enhancements
- [ ] Make layout mobile/tablet friendly
- [ ] Stack sidebar, task cards, and controls vertically on small screens
- [ ] Ensure modals scale or fullscreen on mobile
- [ ] Add responsive navbar (hamburger menu on small screens)

---

_“We deploy not just apps, we deploy legacy.” — Rouge Coders 🥷_
