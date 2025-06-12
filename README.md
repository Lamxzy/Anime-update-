<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Anime News Creator Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .dashboard {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        .panel {
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            backdrop-filter: blur(10px);
        }

        .panel h2 {
            color: #4a5568;
            margin-bottom: 20px;
            font-size: 1.4em;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
        }

        .news-item {
            background: #f8f9fa;
            border-left: 4px solid #667eea;
            padding: 15px;
            margin-bottom: 15px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .news-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .news-item.selected {
            background: #e6f3ff;
            border-left-color: #3182ce;
        }

        .news-title {
            font-weight: bold;
            color: #2d3748;
            margin-bottom: 8px;
        }

        .news-meta {
            font-size: 0.85em;
            color: #718096;
            margin-bottom: 8px;
        }

        .news-summary {
            color: #4a5568;
            line-height: 1.4;
        }

        .content-creator {
            grid-column: 1 / -1;
        }

        .template-selector {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .template-btn {
            padding: 10px 20px;
            border: 2px solid #e2e8f0;
            background: white;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 500;
        }

        .template-btn:hover, .template-btn.active {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .content-preview {
            background: #1a202c;
            color: #e2e8f0;
            padding: 20px;
            border-radius: 10px;
            font-family: 'Courier New', monospace;
            margin-bottom: 20px;
            min-height: 200px;
            white-space: pre-wrap;
        }

        .controls {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            align-items: center;
        }

        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            font-size: 14px;
        }

        .btn-primary {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-secondary {
            background: #e2e8f0;
            color: #4a5568;
        }

        .btn-secondary:hover {
            background: #cbd5e0;
        }

        .input-group {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        .input-group input, .input-group select {
            padding: 8px 12px;
            border: 1px solid #e2e8f0;
            border-radius: 5px;
            font-size: 14px;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-card {
            background: linear-gradient(45deg, #4fd1c7, #38b2ac);
            color: white;
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }

        .stat-number {
            font-size: 1.8em;
            font-weight: bold;
        }

        .stat-label {
            font-size: 0.9em;
            opacity: 0.9;
        }

        .loading {
            display: none;
            text-align: center;
            color: #667eea;
            font-weight: 500;
        }

        .hashtag-suggestions {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-top: 10px;
        }

        .hashtag {
            background: #e6f3ff;
            color: #3182ce;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 0.85em;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .hashtag:hover {
            background: #3182ce;
            color: white;
        }

        @media (max-width: 768px) {
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .controls {
                flex-direction: column;
                align-items: stretch;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎌 Anime News Creator</h1>
            <p>Semi-automated content creation for TikTok</p>
        </div>

        <div class="stats">
            <div class="stat-card">
                <div class="stat-number" id="newsCount">0</div>
                <div class="stat-label">News Items</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" id="contentGenerated">0</div>
                <div class="stat-label">Content Generated</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" id="trending">5</div>
                <div class="stat-label">Trending Topics</div>
            </div>
        </div>

        <div class="dashboard">
            <div class="panel">
                <h2>📰 Latest Anime News</h2>
                <div class="loading" id="newsLoading">Loading latest news...</div>
                <div id="newsFeed">
                    <!-- News items will be populated here -->
                </div>
                <button class="btn btn-secondary" onclick="refreshNews()" style="margin-top: 15px;">
                    🔄 Refresh News
                </button>
                <button class="btn btn-secondary" onclick="addCustomSource()" style="margin-top: 15px; margin-left: 10px;">
                    ➕ Add Source
                </button>
            </div>

            <div class="panel">
                <h2>⚙️ Content Settings</h2>
                <div class="input-group" style="margin-bottom: 15px;">
                    <label>Content Style:</label>
                    <select id="contentStyle">
                        <option value="breaking">🚨 Breaking News</option>
                        <option value="casual">💬 Casual Update</option>
                        <option value="analysis">🔍 Deep Dive</option>
                        <option value="hype">🔥 Hype Beast</option>
                        <option value="comparison">⚡ Quick Facts</option>
                    </select>
                </div>
                <div class="input-group" style="margin-bottom: 15px;">
                    <label>Duration:</label>
                    <select id="duration">
                        <option value="15">15 seconds</option>
                        <option value="30" selected>30 seconds</option>
                        <option value="60">60 seconds</option>
                    </select>
                </div>
                <div class="input-group">
                    <label>Target Audience:</label>
                    <select id="audience">
                        <option value="general">General Anime Fans</option>
                        <option value="hardcore">Hardcore Otaku</option>
                        <option value="casual">Casual Viewers</option>
                        <option value="newcomers">Anime Newcomers</option>
                    </select>
                </div>
            </div>
        </div>

        <div class="panel content-creator">
            <h2>🎬 Content Creator</h2>
            
            <div class="template-selector">
                <div class="template-btn active" onclick="selectTemplate('script')">📝 Script</div>
                <div class="template-btn" onclick="selectTemplate('hooks')">🎣 Hooks</div>
                <div class="template-btn" onclick="selectTemplate('hashtags')">🏷️ Hashtags</div>
                <div class="template-btn" onclick="selectTemplate('thumbnail')">🖼️ Thumbnail Ideas</div>
                <div class="template-btn" onclick="selectTemplate('videoSetup')">🎬 Video Setup</div>
                <div class="template-btn" onclick="selectTemplate('automation')">🤖 Auto Script</div>
            </div>

            <div class="content-preview" id="contentPreview">
Select a news item and click "Generate Content" to create TikTok-ready material!

Your generated content will appear here with:
• Engaging script with timestamps
• Hook variations
• Trending hashtags
• Thumbnail concepts
• Call-to-action suggestions</div>

            <div class="controls">
                <button class="btn btn-primary" onclick="generateContent()">
                    ✨ Generate Content
                </button>
                <button class="btn btn-primary" onclick="generateVideoPackage()" style="background: linear-gradient(45deg, #ff6b6b, #ee5a24);">
                    🎥 Generate Video Package
                </button>
                <button class="btn btn-secondary" onclick="copyContent()">
                    📋 Copy to Clipboard
                </button>
                <button class="btn btn-secondary" onclick="downloadVideoAssets()">
                    📦 Download Assets
                </button>
                <button class="btn btn-secondary" onclick="saveContent()">
                    💾 Save Template
                </button>
                <div class="input-group">
                    <input type="text" id="customKeywords" placeholder="Add custom keywords...">
                    <button class="btn btn-secondary" onclick="addKeywords()">Add</button>
                </div>
            </div>

            <div class="hashtag-suggestions" id="hashtagSuggestions">
                <!-- Hashtag suggestions will appear here -->
            </div>
        </div>
    </div>

    <script>
        let selectedNews = null;
        let currentTemplate = 'script';
        let contentCount = 0;
        let savedTemplates = [];

        // News sources configuration
        const newsSources = [
            {
                name: "Anime News Network",
                rss: "https://www.animenewsnetwork.com/all/rss.xml",
                category: "General",
                priority: 1
            },
            {
                name: "Crunchyroll News",
                rss: "https://www.crunchyroll.com/news/rss",
                category: "Streaming",
                priority: 2
            },
            {
                name: "MyAnimeList News",
                rss: "https://myanimelist.net/rss/news.xml",
                category: "General",
                priority: 3
            }
        ];

        let newsCache = [];
        let lastFetchTime = 0;
        const CACHE_DURATION = 30 * 60 * 1000; // 30 minutes

        function initializeApp() {
            loadNews();
            updateStats();
            generateHashtagSuggestions();
        }

        async function loadNews() {
            const newsFeed = document.getElementById('newsFeed');
            const newsLoading = document.getElementById('newsLoading');
            
            newsLoading.style.display = 'block';
            newsLoading.textContent = 'Fetching latest anime news...';
            
            try {
                // Check if we have cached news that's still fresh
                const now = Date.now();
                if (newsCache.length > 0 && (now - lastFetchTime) < CACHE_DURATION) {
                    displayNews(newsCache);
                    return;
                }

                // Fetch from multiple sources
                const newsPromises = newsSources.map(source => fetchFromSource(source));
                const results = await Promise.allSettled(newsPromises);
                
                let allNews = [];
                results.forEach((result, index) => {
                    if (result.status === 'fulfilled' && result.value) {
                        allNews = allNews.concat(result.value);
                    } else {
                        console.warn(`Failed to fetch from ${newsSources[index].name}:`, result.reason);
                    }
                });

                // Sort by recency and trending indicators
                allNews.sort((a, b) => {
                    if (a.trending && !b.trending) return -1;
                    if (!a.trending && b.trending) return 1;
                    return new Date(b.pubDate) - new Date(a.pubDate);
                });

                // Cache the results
                newsCache = allNews.slice(0, 20); // Keep top 20 news items
                lastFetchTime = now;
                
                displayNews(newsCache);
                
            } catch (error) {
                console.error('Error loading news:', error);
                newsLoading.textContent = 'Error loading news. Using cached data...';
                
                // Fall back to sample data if available
                if (newsCache.length === 0) {
                    newsCache = getSampleNews();
                }
                displayNews(newsCache);
            }
        }

        async function fetchFromSource(source) {
            try {
                // Use a CORS proxy service for RSS feeds
                const proxyUrl = `https://api.allorigins.win/get?url=${encodeURIComponent(source.rss)}`;
                const response = await fetch(proxyUrl);
                const data = await response.json();
                
                // Parse XML content
                const parser = new DOMParser();
                const xml = parser.parseFromString(data.contents, 'text/xml');
                
                const items = xml.querySelectorAll('item');
                const news = [];
                
                items.forEach((item, index) => {
                    if (index < 5) { // Limit to 5 items per source
                        const title = item.querySelector('title')?.textContent || '';
                        const description = item.querySelector('description')?.textContent || '';
                        const pubDate = item.querySelector('pubDate')?.textContent || '';
                        const link = item.querySelector('link')?.textContent || '';
                        
                        // Clean up description (remove HTML tags)
                        const cleanDescription = description.replace(/<[^>]*>/g, '').substring(0, 200);
                        
                        // Determine if trending based on keywords
                        const trendingKeywords = ['breaking', 'announced', 'new', 'season', 'movie', 'release'];
                        const isTrending = trendingKeywords.some(keyword => 
                            title.toLowerCase().includes(keyword) || 
                            cleanDescription.toLowerCase().includes(keyword)
                        );
                        
                        news.push({
                            id: `${source.name}-${index}`,
                            title: title.trim(),
                            summary: cleanDescription.trim(),
                            category: source.category,
                            trending: isTrending,
                            timestamp: formatTimestamp(pubDate),
                            source: source.name,
                            link: link,
                            pubDate: pubDate
                        });
                    }
                });
                
                return news;
            } catch (error) {
                console.error(`Error fetching from ${source.name}:`, error);
                return [];
            }
        }

        function displayNews(newsItems) {
            const newsFeed = document.getElementById('newsFeed');
            const newsLoading = document.getElementById('newsLoading');
            
            newsLoading.style.display = 'none';
            newsFeed.innerHTML = '';
            
            if (newsItems.length === 0) {
                newsFeed.innerHTML = '<div class="news-item">No news available. Please try refreshing.</div>';
                return;
            }
            
            newsItems.forEach(item => {
                const newsElement = document.createElement('div');
                newsElement.className = 'news-item';
                newsElement.onclick = () => selectNews(item);
                
                newsElement.innerHTML = `
                    <div class="news-title">${item.trending ? '🔥 ' : ''}${item.title}</div>
                    <div class="news-meta">${item.category} • ${item.timestamp} • ${item.source}</div>
                    <div class="news-summary">${item.summary}</div>
                `;
                
                newsFeed.appendChild(newsElement);
            });
        }

        function formatTimestamp(pubDate) {
            if (!pubDate) return 'Unknown';
            
            const date = new Date(pubDate);
            const now = new Date();
            const diffInHours = Math.floor((now - date) / (1000 * 60 * 60));
            
            if (diffInHours < 1) return 'Just now';
            if (diffInHours < 24) return `${diffInHours} hours ago`;
            if (diffInHours < 48) return 'Yesterday';
            return `${Math.floor(diffInHours / 24)} days ago`;
        }

        function getSampleNews() {
            // Fallback sample data
            return [
                {
                    id: 1,
                    title: "New Attack on Titan Movie Announced for 2025",
                    summary: "Studio WIT confirms a new Attack on Titan movie focusing on Levi's backstory, set to release in spring 2025.",
                    category: "Movies",
                    trending: true,
                    timestamp: "2 hours ago",
                    source: "Sample Data"
                },
                {
                    id: 2,
                    title: "Demon Slayer Season 4 Gets Release Date",
                    summary: "The highly anticipated fourth season of Demon Slayer will premiere in April 2025, featuring the Infinity Castle arc.",
                    category: "TV Series",
                    trending: true,
                    timestamp: "4 hours ago",
                    source: "Sample Data"
                }
            ];
        }

        function selectNews(newsItem) {
            se