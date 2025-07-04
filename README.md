# news-blog.xdimport React, { useEffect, useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Moon, Sun } from "lucide-react"; import axios from "axios";

const categories = ["All", "Technology", "Sports", "Politics"];

export default function NewsBlog() { const [selectedCategory, setSelectedCategory] = useState("All"); const [articles, setArticles] = useState([]); const [darkMode, setDarkMode] = useState(false);

useEffect(() => { fetchArticles(); }, []);

const fetchArticles = async () => { try { const res = await axios.get("https://newsapi.org/v2/top-headlines?country=in&apiKey=YOUR_API_KEY"); const formattedArticles = res.data.articles.map((article, index) => ({ id: index, title: article.title, summary: article.description, image: article.urlToImage, category: "General", })); setArticles(formattedArticles); } catch (error) { console.error("Failed to fetch news", error); } };

const filteredArticles = selectedCategory === "All" ? articles : articles.filter((a) => a.category === selectedCategory);

return ( <div className={darkMode ? "dark bg-gray-900 text-white min-h-screen p-6" : "bg-white text-black min-h-screen p-6"}> <div className="max-w-7xl mx-auto"> <div className="flex justify-between items-center mb-6"> <h1 className="text-4xl font-bold">🗞️ Daily India News</h1> <Button variant="ghost" onClick={() => setDarkMode(!darkMode)}> {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />} </Button> </div>

{/* Category Filter */}
    <div className="flex flex-wrap justify-center gap-3 mb-6">
      {categories.map((cat) => (
        <Button
          key={cat}
          variant={selectedCategory === cat ? "default" : "outline"}
          onClick={() => setSelectedCategory(cat)}
        >
          {cat}
        </Button>
      ))}
    </div>

    {/* Articles */}
    <div className="grid sm:grid-cols-2 lg:grid-cols-3 gap-6">
      {filteredArticles.map((article) => (
        <Card key={article.id} className="overflow-hidden shadow-md">
          <img
            src={article.image || "https://source.unsplash.com/600x400/?news"}
            alt={article.title}
            className="w-full h-48 object-cover"
          />
          <CardContent className="p-4">
            <h2 className="text-xl font-semibold mb-2">{article.title}</h2>
            <p className="text-sm text-gray-600 dark:text-gray-300 mb-2">
              {article.summary}
            </p>
            <span className="text-xs bg-blue-100 text-blue-800 px-2 py-1 rounded">
              {article.category}
            </span>
          </CardContent>
        </Card>
      ))}
    </div>
  </div>
</div>

); }

