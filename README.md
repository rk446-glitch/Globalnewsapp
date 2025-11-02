# Globalnewsapp
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>News Galaxy 🌌</title>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

    body {
        margin: 0;
        font-family: 'Poppins', sans-serif;
        background: radial-gradient(circle at top left, #0f0c29, #302b63, #24243e);
        color: white;
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-x: hidden;
    }

    .container {
        text-align: center;
        width: 90%;
        max-width: 1000px;
    }

    h1 {
        font-size: 2.5rem;
        color: #fff;
        text-shadow: 0 0 10px #6a11cb, 0 0 20px #2575fc;
    }

    /* Login Section */
    #login-section {
        background: rgba(255,255,255,0.1);
        padding: 40px;
        border-radius: 15px;
        box-shadow: 0 0 20px rgba(255,255,255,0.2);
        backdrop-filter: blur(10px);
        width: 100%;
        max-width: 400px;
    }

    input {
        width: 80%;
        padding: 12px;
        margin: 10px 0;
        border: none;
        border-radius: 8px;
        outline: none;
        font-size: 1rem;
    }

    button {
        padding: 12px 25px;
        font-size: 1rem;
        color: white;
        background: linear-gradient(45deg, #6a11cb, #2575fc);
        border: none;
        border-radius: 8px;
        cursor: pointer;
        box-shadow: 0 0 10px #2575fc;
        transition: 0.3s;
    }

    button:hover {
        transform: scale(1.05);
        box-shadow: 0 0 20px #6a11cb;
    }

    /* News Section */
    #news-section {
        display: none;
        flex-direction: column;
        align-items: center;
        width: 100%;
        max-width: 1200px;
        margin-top: 20px;
    }

    #search-bar {
        margin: 20px 0;
        display: flex;
        justify-content: center;
        gap: 10px;
        flex-wrap: wrap;
    }

    #search-input {
        width: 60%;
        max-width: 400px;
    }

    #news-container {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
        gap: 20px;
        width: 100%;
    }

    .news-card {
        background: rgba(255,255,255,0.08);
        border-radius: 12px;
        overflow: hidden;
        box-shadow: 0 0 15px rgba(255,255,255,0.1);
        transition: transform 0.3s, box-shadow 0.3s;
    }

    .news-card:hover {
        transform: translateY(-5px);
        box-shadow: 0 0 25px rgba(138,43,226,0.5);
    }

    .news-card img {
        width: 100%;
        height: 180px;
        object-fit: cover;
    }

    .news-content {
        padding: 15px;
        text-align: left;
    }

    .news-content h3 {
        margin: 5px 0;
        font-size: 1.1rem;
        color: #fff;
    }

    .news-content p {
        color: #ccc;
        font-size: 0.9rem;
    }

    .source {
        font-size: 0.8rem;
        color: #aaa;
        margin-top: 10px;
    }

    .footer {
        margin-top: 40px;
        color: #aaa;
        font-size: 0.9rem;
    }
</style>
</head>

<body>
    <div class="container">
        <!-- LOGIN SECTION -->
        <div id="login-section">
            <h1>News Galaxy 🌌</h1>
            <p>Login with your phone number to explore galaxy of news!</p>
            <input type="text" id="phone-input" placeholder="Enter phone number" />
            <button id="get-otp-btn">Get OTP</button>
            <div id="otp-section" style="display:none;">
                <input type="text" id="otp-input" placeholder="Enter OTP (1234)" />
                <button id="verify-btn">Verify OTP</button>
            </div>
            <p id="login-error" style="color:red;"></p>
        </div>

        <!-- NEWS SECTION -->
        <div id="news-section">
            <h1>🪐 News Galaxy</h1>
            <div id="search-bar">
                <input type="text" id="search-input" placeholder="Search by keyword (e.g. sports, tech...)" />
                <button id="search-btn">Search</button>
            </div>
            <div id="news-container"></div>
            <div class="footer">© 2025 News Galaxy | Powered by NewsData.io</div>
        </div>
    </div>

<script>
    const loginSection = document.getElementById("login-section");
    const newsSection = document.getElementById("news-section");
    const getOtpBtn = document.getElementById("get-otp-btn");
    const verifyBtn = document.getElementById("verify-btn");
    const otpSection = document.getElementById("otp-section");
    const loginError = document.getElementById("login-error");

    let mockOtp = "1234";

    getOtpBtn.addEventListener("click", () => {
        const phone = document.getElementById("phone-input").value.trim();
        if (phone.length < 10) {
            loginError.textContent = "Please enter a valid phone number.";
            return;
        }
        loginError.textContent = "";
        otpSection.style.display = "block";
    });

    verifyBtn.addEventListener("click", () => {
        const otp = document.getElementById("otp-input").value.trim();
        if (otp === mockOtp) {
            loginSection.style.display = "none";
            newsSection.style.display = "flex";
            loadNews("india"); // Default load
        } else {
            loginError.textContent = "Incorrect OTP. Try again.";
        }
    });

    // News Loading
    const searchBtn = document.getElementById("search-btn");
    const searchInput = document.getElementById("search-input");
    const newsContainer = document.getElementById("news-container");

    const API_KEY = "pub_d9d5ff66ab1041f591c531de0dff8ddd";

    async function loadNews(keyword) {
        newsContainer.innerHTML = "<p>Loading news...</p>";
        try {
            const url = `https://newsdata.io/api/1/latest?apikey=${API_KEY}&q=${encodeURIComponent(keyword)}`;
            const response = await fetch(url);
            const data = await response.json();

            if (!data.results || data.results.length === 0) {
                newsContainer.innerHTML = "<p>No news found.</p>";
                return;
            }

            newsContainer.innerHTML = "";
            data.results.forEach(article => {
                const card = document.createElement("div");
                card.className = "news-card";

                const image = article.image_url || "https://via.placeholder.com/400x200?text=No+Image";
                const title = article.title || "No title available";
                const desc = article.description || "No description available";
                const link = article.link || "#";
                const source = article.source_id || "Unknown";
                const date = article.pubDate ? new Date(article.pubDate).toLocaleString() : "";

                card.innerHTML = `
                    <a href="${link}" target="_blank" style="text-decoration:none;color:inherit;">
                        <img src="${image}" alt="news image">
                        <div class="news-content">
                            <h3>${title}</h3>
                            <p>${desc}</p>
                            <div class="source">🌐 ${source} | 🕒 ${date}</div>
                        </div>
                    </a>
                `;
                newsContainer.appendChild(card);
            });
        } catch (error) {
            newsContainer.innerHTML = "<p>Error loading news. Try again later.</p>";
            console.error(error);
        }
    }

    searchBtn.addEventListener("click", () => {
        const keyword = searchInput.value.trim() || "india";
        loadNews(keyword);
    });
</script>

</body>
</html>



