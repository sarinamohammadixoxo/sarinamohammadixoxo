"use client"

import { useEffect, useRef, useState } from "react"
import { Card, CardContent } from "@/components/ui/card"
import { Play } from "lucide-react"

const videos = [
  {
    title: "The Future of Leadership",
    thumbnail: "/leadership-conference-presentation.jpg",
    embedId: "dQw4w9WgXcQ",
  },
  {
    title: "Innovation in the Digital Age",
    thumbnail: "/tech-keynote-speech.jpg",
    embedId: "dQw4w9WgXcQ",
  },
  {
    title: "Empowering Teams for Success",
    thumbnail: "/business-motivational-speaker.jpg",
    embedId: "dQw4w9WgXcQ",
  },
]

export default function Videos() {
  const sectionRef = useRef<HTMLElement>(null)
  const [isVisible, setIsVisible] = useState(false)
  const [selectedVideo, setSelectedVideo] = useState<string | null>(null)

  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsVisible(true)
        }
      },
      { threshold: 0.1 },
    )

    if (sectionRef.current) {
      observer.observe(sectionRef.current)
    }

    return () => {
      if (sectionRef.current) {
        observer.unobserve(sectionRef.current)
      }
    }
  }, [])

  return (
    <section id="videos" ref={sectionRef} className="py-24 md:py-32 bg-background">
      <div className="container mx-auto px-4">
        <div
          className={`text-center mb-16 transition-all duration-1000 ${
            isVisible ? "opacity-100 translate-y-0" : "opacity-0 translate-y-10"
          }`}
        >
          <h2 className="text-4xl md:text-5xl lg:text-6xl font-serif font-bold text-primary mb-6 text-balance">
            {"Featured Talks"}
          </h2>
          <p className="text-xl text-foreground/80 max-w-2xl mx-auto">
            {"Watch highlights from recent keynotes and presentations"}
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 max-w-6xl mx-auto">
          {videos.map((video, index) => (
            <div
              key={index}
              className={`transition-all duration-700 ${
                isVisible ? "opacity-100 translate-y-0" : "opacity-0 translate-y-10"
              }`}
              style={{ transitionDelay: `${index * 150}ms` }}
            >
              <Card className="overflow-hidden border-0 shadow-xl hover:shadow-2xl transition-shadow duration-300 group cursor-pointer">
                <div className="relative h-56 overflow-hidden" onClick={() => setSelectedVideo(video.embedId)}>
                  <img
                    src={video.thumbnail || "/placeholder.svg"}
                    alt={video.title}
                    className="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
                  />
                  <div className="absolute inset-0 bg-primary/60 group-hover:bg-primary/40 transition-colors duration-300 flex items-center justify-center">
                    <div className="bg-white rounded-full p-4 transform group-hover:scale-110 transition-transform duration-300">
                      <Play className="h-8 w-8 text-primary fill-primary" />
                    </div>
                  </div>
                </div>
                <CardContent className="p-6 bg-card">
                  <h3 className="text-lg font-bold text-card-foreground text-balance">{video.title}</h3>
                </CardContent>
              </Card>
            </div>
          ))}
        </div>

        {/* Video Modal */}
        {selectedVideo && (
          <div
            className="fixed inset-0 bg-primary/95 z-50 flex items-center justify-center p-4"
            onClick={() => setSelectedVideo(null)}
          >
            <div className="max-w-4xl w-full" onClick={(e) => e.stopPropagation()}>
              <div className="relative pb-[56.25%] h-0">
                <iframe
                  className="absolute top-0 left-0 w-full h-full rounded-lg"
                  src={`https://www.youtube.com/embed/${selectedVideo}?autoplay=1`}
                  title="YouTube video player"
                  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                  allowFullScreen
                />
              </div>
              <button onClick={() => setSelectedVideo(null)} className="mt-4 text-white hover:text-white/80 text-lg">
                {"Close"}
              </button>
            </div>
          </div>
        )}
      </div>
    </section>
  )
}
