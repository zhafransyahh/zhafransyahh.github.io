# How to Build Your Own Portfolio Website (No Coding Experience Needed!)

Hi! This guide will show you how to build a professional portfolio website just like mine, step-by-step. Don't worry if you've never coded before—I'll explain everything in simple terms.

---

## What You'll Need

1. **A GitHub Account** (free) — Think of this as a free storage locker for your website
2. **A Text Editor** (free) — We'll use one to write the website code (it's simpler than it sounds!)
3. **About 1-2 hours** to get started

That's it! No fancy tools or expensive software.

---

## Part 1: Set Up Your Free Website Hosting (GitHub Pages)

### Step 1: Create a GitHub Account

1. Go to [github.com](https://github.com)
2. Click "Sign up"
3. Create a username (this will be part of your website URL), email, and password
4. Verify your email

### Step 2: Create Your Portfolio Repository

1. After signing in, click the **+** icon in the top-right corner
2. Select **New repository**
3. Name it exactly: `YOUR_USERNAME.github.io`
   - Replace `YOUR_USERNAME` with your actual GitHub username
   - For example: if your username is `john-doe`, name it `john-doe.github.io`
4. Add a description: "My Portfolio Website"
5. Make it **Public** (so others can see it)
6. Click **Create repository**

### Step 3: Turn It Into a Live Website

1. Go to your repository's **Settings** tab
2. Scroll down to **GitHub Pages**
3. Under "Branch", select **main** and click **Save**
4. Wait a minute, then go to: `https://YOUR_USERNAME.github.io`
5. You should see a blank page—that's perfect! Your website is live.

---

## Part 2: The Building Blocks (Super Simple!)

Before we build, let me explain the three languages we use (don't worry, they're simple):

### HTML — The Structure (Like Writing an Outline)
```html
<h1>My Name</h1>
<p>I'm a designer.</p>
```

- `<h1>` means "big heading"
- `<p>` means "paragraph" or regular text
- Everything between `<` and `>` is an instruction

### CSS — The Styling (Like Choosing Colors & Fonts)
```css
h1 {
  color: blue;
  font-size: 32px;
}
```

- This makes all `<h1>` headings blue and larger
- Think of it as a rulebook for how things look

### JavaScript — The Interactivity (Like Adding Buttons That Do Things)
```javascript
button.addEventListener('click', function() {
  alert('You clicked me!');
});
```

- This makes things respond to clicks or scrolling
- We'll keep it minimal—you don't need much to get started

---

## Part 3: Build Your First Portfolio Page

### Option A: Use a Template (Easiest)

I'll provide you a basic template below. You just need to replace the words with your own information.

### Option B: Build From Scratch (Skip This for Now)

If you want to understand how it all works, see Part 5.

---

## Part 4: Your First Template (Copy & Paste)

### Step 1: Create Your Home Page File

1. Go to your repository on GitHub
2. Click **Add file → Create new file**
3. Name it: `index.html`
4. Copy and paste the code below:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Name - Portfolio</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            color: #333;
            background-color: #f4f4f4;
        }

        header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 60px 20px;
            text-align: center;
        }

        header h1 {
            font-size: 48px;
            margin-bottom: 10px;
        }

        header p {
            font-size: 20px;
            opacity: 0.9;
        }

        nav {
            background-color: #333;
            padding: 15px;
            text-align: center;
            position: sticky;
            top: 0;
        }

        nav a {
            color: white;
            text-decoration: none;
            margin: 0 20px;
            font-size: 16px;
        }

        nav a:hover {
            text-decoration: underline;
        }

        .container {
            max-width: 1000px;
            margin: 40px auto;
            padding: 0 20px;
        }

        section {
            background: white;
            padding: 30px;
            margin: 20px 0;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        section h2 {
            color: #667eea;
            margin-bottom: 20px;
            border-bottom: 2px solid #667eea;
            padding-bottom: 10px;
        }

        .project {
            margin: 20px 0;
            padding: 15px;
            border-left: 4px solid #667eea;
            background-color: #f9f9f9;
        }

        .project h3 {
            color: #764ba2;
            margin-bottom: 5px;
        }

        .project p {
            margin: 5px 0;
            color: #666;
        }

        footer {
            background-color: #333;
            color: white;
            text-align: center;
            padding: 20px;
            margin-top: 40px;
        }

        footer a {
            color: #667eea;
            text-decoration: none;
        }

        footer a:hover {
            text-decoration: underline;
        }

        @media (max-width: 768px) {
            header h1 {
                font-size: 32px;
            }

            nav a {
                display: block;
                margin: 10px 0;
            }
        }
    </style>
</head>
<body>
    <!-- Header / Hero Section -->
    <header>
        <h1>Your Name Here</h1>
        <p>Your Job Title or Tagline</p>
    </header>

    <!-- Navigation -->
    <nav>
        <a href="#about">About</a>
        <a href="#experience">Experience</a>
        <a href="#projects">Projects</a>
        <a href="#contact">Contact</a>
    </nav>

    <!-- Main Content -->
    <div class="container">
        <!-- About Section -->
        <section id="about">
            <h2>About Me</h2>
            <p>
                Write a brief introduction about yourself here.
                What do you do? What are you passionate about?
                Keep it to 2-3 sentences to start.
            </p>
        </section>

        <!-- Experience Section -->
        <section id="experience">
            <h2>Experience</h2>
            <div class="project">
                <h3>Your Job Title</h3>
                <p><strong>Company Name</strong> | 2022 - Present</p>
                <p>Describe what you did or learned in this role. What were your main responsibilities?</p>
            </div>
            <div class="project">
                <h3>Previous Job Title</h3>
                <p><strong>Previous Company</strong> | 2020 - 2022</p>
                <p>Describe what you did or learned in this role.</p>
            </div>
        </section>

        <!-- Projects Section -->
        <section id="projects">
            <h2>Projects & Portfolio</h2>
            <div class="project">
                <h3>Project One</h3>
                <p><strong>Role:</strong> Designer / Developer / Manager</p>
                <p>Describe the project. What problem did it solve? What's the result?</p>
            </div>
            <div class="project">
                <h3>Project Two</h3>
                <p><strong>Role:</strong> Your role on the project</p>
                <p>Describe what this project was about and what you accomplished.</p>
            </div>
        </section>

        <!-- Skills Section -->
        <section id="skills">
            <h2>Skills</h2>
            <p>
                <strong>Skills:</strong> Skill 1, Skill 2, Skill 3, Skill 4, Skill 5
            </p>
            <p>
                <strong>Tools:</strong> Tool 1, Tool 2, Tool 3, Tool 4
            </p>
        </section>

        <!-- Contact Section -->
        <section id="contact">
            <h2>Get In Touch</h2>
            <p>Email: <a href="mailto:your.email@example.com">your.email@example.com</a></p>
            <p>LinkedIn: <a href="https://linkedin.com/in/yourprofile" target="_blank">linkedin.com/in/yourprofile</a></p>
            <p>GitHub: <a href="https://github.com/yourprofile" target="_blank">github.com/yourprofile</a></p>
        </section>
    </div>

    <!-- Footer -->
    <footer>
        <p>&copy; 2024 Your Name. All rights reserved.</p>
    </footer>
</body>
</html>
```

### Step 2: Customize Your Content

1. Click the pencil icon to edit the file
2. Replace these placeholders with your own info:
   - `Your Name Here` → Your actual name
   - `Your Job Title or Tagline` → Your profession or tagline
   - `Your email@example.com` → Your real email
   - Add your actual experience and projects

### Step 3: Save and Publish

1. Click **Commit changes**
2. Add a message like: "Add initial portfolio page"
3. Wait 1-2 minutes
4. Go to `https://YOUR_USERNAME.github.io`
5. **Your website is live!** 🎉

---

## Part 5: Making It Your Own (Customization)

### Change the Colors

Find this section near the top of your HTML file:

```css
header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    ...
}
```

Change `#667eea` and `#764ba2` to different color codes. Find color codes at [colorhexa.com](https://www.colorhexa.com).

Popular color combinations:
- Professional blue: `#1e40af` and `#1e3a8a`
- Warm orange: `#ea580c` and `#c2410c`
- Dark & modern: `#0d0d0c` and `#f0ede6`

### Change the Font

Find this line:

```css
font-family: Arial, sans-serif;
```

Change it to:
- `Georgia, serif` — Classic, elegant
- `Courier New, monospace` — Technical-looking
- `Trebuchet MS, sans-serif` — Clean and modern

### Add a Profile Picture

1. Go to your repository on GitHub
2. Click **Add file → Upload files**
3. Upload a photo named `profile.jpg`
4. In your HTML, find the line after `<header>` and add:

```html
<img src="profile.jpg" alt="Profile Picture" style="width: 150px; height: 150px; border-radius: 50%; margin-top: 20px;">
```

---

## Part 6: Going Deeper (For the Curious)

### Add More Pages

Want an "About" page? Create a new file:

1. Click **Add file → Create new file**
2. Name it: `about.html`
3. Copy the template above and modify it
4. Update the navigation in `index.html`:

```html
<nav>
    <a href="index.html">Home</a>
    <a href="about.html">About</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
</nav>
```

### Add a Contact Form

Replace the Contact section with:

```html
<section id="contact">
    <h2>Get In Touch</h2>
    <form action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
        <input type="text" name="name" placeholder="Your Name" required>
        <input type="email" name="email" placeholder="Your Email" required>
        <textarea name="message" placeholder="Your Message" rows="5" required></textarea>
        <button type="submit">Send</button>
    </form>
</section>
```

Then go to [formspree.io](https://formspree.io) to get a form ID (takes 2 minutes).

---

## Part 7: Troubleshooting

### My website isn't showing up
- **Solution:** Wait 5 minutes, then hard-refresh (Ctrl+Shift+R or Cmd+Shift+R)
- Check that your repository is named exactly `YOUR_USERNAME.github.io` (all lowercase)

### My changes aren't appearing
- **Solution:** Go back to your repository, edit the file again, and commit the changes
- Changes take 1-2 minutes to appear on your website

### My website looks broken or weird
- **Solution:** Check for missing `>` symbols or typos in the HTML
- Use your browser's developer tools (F12) to see error messages

---

## Part 8: Next Steps

### Want to make it even better?

1. **Add animations** — Learn CSS animations on [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)
2. **Add a blog** — Learn about static site generators like [Jekyll](https://jekyllrb.com/)
3. **Buy a custom domain** — Purchase from GoDaddy or Namecheap, then connect to GitHub Pages
4. **Learn more coding** — Try free courses on [freeCodeCamp](https://freecodecamp.org) or [Codecademy](https://codecademy.com)

---

## Quick Tips

✅ **Do:**
- Keep your content clear and concise
- Update regularly with new projects
- Use professional photos and descriptions
- Test on your phone to make sure it looks good

❌ **Don't:**
- Use too many colors or fonts (stick to 2-3)
- Add too much text (keep sections brief)
- Forget to spell-check your bio
- Leave placeholder text on your live site

---

## Resources

- [GitHub Pages Documentation](https://pages.github.com/)
- [HTML Basics](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [CSS Basics](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [Color Picker](https://www.colorhexa.com)
- [Free Images](https://unsplash.com) & [Pexels](https://pexels.com)

---

## Questions?

If you get stuck:
1. Try searching your error on [Stack Overflow](https://stackoverflow.com)
2. Check the [GitHub Pages troubleshooting guide](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site)
3. Ask in a community like [Dev.to](https://dev.to) or Reddit's r/webdev

---

**You've got this! 🚀**
