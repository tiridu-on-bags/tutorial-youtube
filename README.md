# tutorial-youtube
## Se muestran los códigos iniciales que se me fueron dados, modificalos a tu gusto siguiendo el tutorial 
### about-section.tsx

import { useSectionIntersect } from "@/hooks/use-section-intersect";
import { motion } from "framer-motion";
import { Card, CardContent } from "@/components/ui/card";
import { Mail, Download } from "lucide-react";

export default function AboutSection() {
  const { ref, isInView } = useSectionIntersect();

  return (
    <section 
      id="about" 
      ref={ref} 
      className="py-20 bg-background"
    >
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        <motion.div 
          className="max-w-3xl mx-auto mb-12 text-center"
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.7 }}
        >
          <h2 className="text-3xl sm:text-4xl font-bold mb-4">About Me</h2>
          <div className="h-1 w-20 bg-primary mx-auto mb-6"></div>
          <p className="text-lg text-muted-foreground">
            Get to know the person behind the code
          </p>
        </motion.div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-12 items-center">
          <motion.div 
            className="order-2 md:order-1"
            initial={{ opacity: 0, x: -20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: -20 }}
            transition={{ duration: 0.7, delay: 0.2 }}
          >
            <h3 className="text-2xl font-semibold mb-4 text-primary">Who I Am</h3>
            <p className="text-foreground mb-6 leading-relaxed">
              Hello! I'm a passionate web developer specializing in building beautiful, functional applications using modern technologies like React. With a background in both design and development, I bridge the gap between aesthetics and functionality.
            </p>
            
            <h3 className="text-2xl font-semibold mb-4 text-primary">What I Do</h3>
            <p className="text-foreground mb-6 leading-relaxed">
              I create responsive, accessible, and performant web applications that provide exceptional user experiences. My approach focuses on clean code, modern design patterns, and staying current with the evolving web ecosystem.
            </p>
            
            <div className="flex flex-wrap gap-3 mt-6">
              <span className="px-3 py-1 bg-primary/10 text-primary rounded-full text-sm">Frontend Development</span>
              <span className="px-3 py-1 bg-primary/10 text-primary rounded-full text-sm">React</span>
              <span className="px-3 py-1 bg-primary/10 text-primary rounded-full text-sm">UI/UX Design</span>
              <span className="px-3 py-1 bg-primary/10 text-primary rounded-full text-sm">Responsive Design</span>
            </div>
          </motion.div>
          
          <motion.div 
            className="order-1 md:order-2 relative"
            initial={{ opacity: 0, x: 20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: 20 }}
            transition={{ duration: 0.7, delay: 0.4 }}
          >
            <div className="relative z-10 rounded-lg overflow-hidden shadow-xl border">
              <div className="aspect-video w-full bg-muted/70 relative flex items-center justify-center">
                <svg 
                  className="w-1/2 h-1/2 text-muted-foreground/30" 
                  xmlns="http://www.w3.org/2000/svg" 
                  viewBox="0 0 24 24" 
                  fill="none" 
                  stroke="currentColor" 
                  strokeWidth="1" 
                  strokeLinecap="round" 
                  strokeLinejoin="round"
                >
                  <path d="M12 12m-10 0a10 10 0 1 0 20 0a10 10 0 1 0 -20 0" />
                  <path d="M12 8l.01 0" />
                  <path d="M11 12l1 0l0 4l1 0" />
                </svg>
              </div>
              <CardContent className="p-6">
                <h4 className="font-semibold text-xl mb-2">My Development Journey</h4>
                <p className="text-muted-foreground">
                  With over 5 years of experience in web development, I've worked on various projects ranging from e-commerce platforms to complex web applications.
                </p>
                <div className="mt-4 flex gap-4">
                  <a href="#contact" className="text-primary hover:text-primary/80 flex items-center text-sm font-medium">
                    <Mail className="mr-1 h-4 w-4" /> Contact
                  </a>
                  <a href="#" className="text-primary hover:text-primary/80 flex items-center text-sm font-medium">
                    <Download className="mr-1 h-4 w-4" /> Resume
                  </a>
                </div>
              </CardContent>
            </div>
            
            {/* Decorative elements */}
            <div className="absolute -top-4 -right-4 w-40 h-40 bg-primary/10 rounded-lg -z-10 transform rotate-6"></div>
            <div className="absolute -bottom-4 -left-4 w-40 h-40 bg-pink-500/10 rounded-lg -z-10 transform -rotate-6"></div>
          </motion.div>
        </div>
      </div>
    </section>
  );
}

### Footer footer-section.tsx
import { useSectionIntersect } from "@/hooks/use-section-intersect";
import { motion } from "framer-motion";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";
import { insertMessageSchema } from "@shared/schema";
import { apiRequest } from "@/lib/queryClient";
import { useToast } from "@/hooks/use-toast";
import { useMutation } from "@tanstack/react-query";
import { Card } from "@/components/ui/card";
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Button } from "@/components/ui/button";
import { Mail, MapPin, Link as LinkIcon, Send, Calendar } from "lucide-react";

// Add email validation to the schema
const contactFormSchema = insertMessageSchema.extend({
  email: z.string().email("Please enter a valid email address"),
  message: z.string().min(10, "Message must be at least 10 characters"),
});

type ContactFormValues = z.infer<typeof contactFormSchema>;

export default function ContactSection() {
  const { ref, isInView } = useSectionIntersect();
  const { toast } = useToast();

  // Set up form with react-hook-form and zod validation
  const form = useForm<ContactFormValues>({
    resolver: zodResolver(contactFormSchema),
    defaultValues: {
      name: "",
      email: "",
      subject: "",
      message: "",
    },
  });

  // Set up the form submission mutation
  const mutation = useMutation({
    mutationFn: async (data: ContactFormValues) => {
      const response = await apiRequest("POST", "/api/contact", data);
      return response.json();
    },
    onSuccess: () => {
      toast({
        title: "Message sent!",
        description: "Thanks for reaching out. I'll get back to you soon.",
        variant: "default",
      });
      form.reset();
    },
    onError: (error) => {
      toast({
        title: "Something went wrong",
        description: error instanceof Error ? error.message : "Failed to send your message. Please try again.",
        variant: "destructive",
      });
    },
  });

  function onSubmit(data: ContactFormValues) {
    mutation.mutate(data);
  }

  return (
    <section 
      id="contact" 
      ref={ref} 
      className="py-20 bg-muted/50"
    >
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        <motion.div 
          className="max-w-3xl mx-auto mb-12 text-center"
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.7 }}
        >
          <h2 className="text-3xl sm:text-4xl font-bold mb-4">Get In Touch</h2>
          <div className="h-1 w-20 bg-primary mx-auto mb-6"></div>
          <p className="text-lg text-muted-foreground">
            Have a project in mind? Let's discuss how I can help bring your ideas to life.
          </p>
        </motion.div>

        <div className="max-w-4xl mx-auto">
          <div className="grid grid-cols-1 md:grid-cols-5 gap-8">
            {/* Contact Info */}
            <motion.div 
              className="md:col-span-2 space-y-6"
              initial={{ opacity: 0, x: -20 }}
              animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: -20 }}
              transition={{ duration: 0.7, delay: 0.2 }}
            >
              <Card className="bg-background p-6">
                <h3 className="text-xl font-semibold mb-6 text-primary">Contact Information</h3>
                
                <div className="space-y-4">
                  <div className="flex items-start">
                    <div className="flex-shrink-0 h-10 w-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                      <Mail className="h-5 w-5" />
                    </div>
                    <div className="ml-4">
                      <h4 className="text-sm font-semibold text-muted-foreground">Email</h4>
                      <a href="mailto:contact@example.com" className="text-foreground hover:text-primary transition-colors">
                        contact@example.com
                      </a>
                    </div>
                  </div>
                  
                  <div className="flex items-start">
                    <div className="flex-shrink-0 h-10 w-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                      <MapPin className="h-5 w-5" />
                    </div>
                    <div className="ml-4">
                      <h4 className="text-sm font-semibold text-muted-foreground">Location</h4>
                      <p className="text-foreground">San Francisco, California</p>
                    </div>
                  </div>
                  
                  <div className="flex items-start">
                    <div className="flex-shrink-0 h-10 w-10 rounded-full bg-primary/10 flex items-center justify-center text-primary">
                      <LinkIcon className="h-5 w-5" />
                    </div>
                    <div className="ml-4">
                      <h4 className="text-sm font-semibold text-muted-foreground">Social</h4>
                      <div className="flex space-x-3 mt-2">
                        <a href="https://github.com" className="text-muted-foreground hover:text-primary transition-colors">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                            <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22" />
                          </svg>
                        </a>
                        <a href="https://linkedin.com" className="text-muted-foreground hover:text-primary transition-colors">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                            <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z" />
                            <rect x="2" y="9" width="4" height="12" />
                            <circle cx="4" cy="4" r="2" />
                          </svg>
                        </a>
                        <a href="https://twitter.com" className="text-muted-foreground hover:text-primary transition-colors">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                            <path d="M22 4s-.7 2.1-2 3.4c1.6 10-9.4 17.3-18 11.6 2.2.1 4.4-.6 6-2C3 15.5.5 9.6 3 5c2.2 2.6 5.6 4.1 9 4-.9-4.2 4-6.6 7-3.8 1.1 0 3-1.2 3-1.2z" />
                          </svg>
                        </a>
                      </div>
                    </div>
                  </div>
                </div>
              </Card>

              <Card className="bg-background p-6">
                <h3 className="text-xl font-semibold mb-4 text-primary">Schedule a Call</h3>
                <p className="text-muted-foreground mb-4">Prefer to talk directly? Book a 30-minute consultation to discuss your project.</p>
                <Button className="gap-2">
                  Schedule Call
                  <Calendar className="h-4 w-4" />
                </Button>
              </Card>
            </motion.div>

            {/* Contact Form */}
            <motion.div 
              className="md:col-span-3 bg-background p-6 rounded-lg shadow-sm border"
              initial={{ opacity: 0, x: 20 }}
              animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: 20 }}
              transition={{ duration: 0.7, delay: 0.4 }}
            >
              <h3 className="text-xl font-semibold mb-6 text-primary">Send a Message</h3>
              
              <Form {...form}>
                <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
                  <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <FormField
                      control={form.control}
                      name="name"
                      render={({ field }) => (
                        <FormItem>
                          <FormLabel>Name</FormLabel>
                          <FormControl>
                            <Input placeholder="Your name" {...field} />
                          </FormControl>
                          <FormMessage />
                        </FormItem>
                      )}
                    />
                    
                    <FormField
                      control={form.control}
                      name="email"
                      render={({ field }) => (
                        <FormItem>
                          <FormLabel>Email</FormLabel>
                          <FormControl>
                            <Input placeholder="Your email" {...field} />
                          </FormControl>
                          <FormMessage />
                        </FormItem>
                      )}
                    />
                  </div>
                  
                  <FormField
                    control={form.control}
                    name="subject"
                    render={({ field }) => (
                      <FormItem>
                        <FormLabel>Subject</FormLabel>
                        <FormControl>
                          <Input placeholder="Subject of your message" {...field} />
                        </FormControl>
                        <FormMessage />
                      </FormItem>
                    )}
                  />
                  
                  <FormField
                    control={form.control}
                    name="message"
                    render={({ field }) => (
                      <FormItem>
                        <FormLabel>Message</FormLabel>
                        <FormControl>
                          <Textarea 
                            placeholder="Your message" 
                            rows={5} 
                            {...field} 
                          />
                        </FormControl>
                        <FormMessage />
                      </FormItem>
                    )}
                  />
                  
                  <Button 
                    type="submit" 
                    className="w-full gap-2"
                    disabled={mutation.isPending}
                  >
                    {mutation.isPending ? "Sending..." : "Send Message"}
                    <Send className="h-4 w-4" />
                  </Button>
                </form>
              </Form>
            </motion.div>
          </div>
        </div>
      </div>
    </section>
  );
}

### 
import { useEffect, useState } from "react";

export default function Footer() {
  const [currentYear, setCurrentYear] = useState(new Date().getFullYear());
  
  useEffect(() => {
    setCurrentYear(new Date().getFullYear());
  }, []);

  return (
    <footer className="bg-gray-900 text-gray-400 py-12">
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        <div className="flex flex-col md:flex-row justify-between items-center">
          <div className="mb-6 md:mb-0">
            <a href="#hero" className="text-xl font-bold text-white flex items-center">
              Portfolio
            </a>
            <p className="mt-2 max-w-md">Building modern web experiences with React and a passion for clean design.</p>
          </div>
          
          <div className="flex flex-col items-center md:items-end">
            <div className="flex space-x-4 mb-4">
              <a href="https://github.com" className="text-gray-400 hover:text-white transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                  <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22" />
                </svg>
              </a>
              <a href="https://linkedin.com" className="text-gray-400 hover:text-white transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                  <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z" />
                  <rect x="2" y="9" width="4" height="12" />
                  <circle cx="4" cy="4" r="2" />
                </svg>
              </a>
              <a href="https://twitter.com" className="text-gray-400 hover:text-white transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                  <path d="M22 4s-.7 2.1-2 3.4c1.6 10-9.4 17.3-18 11.6 2.2.1 4.4-.6 6-2C3 15.5.5 9.6 3 5c2.2 2.6 5.6 4.1 9 4-.9-4.2 4-6.6 7-3.8 1.1 0 3-1.2 3-1.2z" />
                </svg>
              </a>
              <a href="https://instagram.com" className="text-gray-400 hover:text-white transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="h-5 w-5">
                  <rect x="2" y="2" width="20" height="20" rx="5" ry="5" />
                  <path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z" />
                  <line x1="17.5" y1="6.5" x2="17.51" y2="6.5" />
                </svg>
              </a>
            </div>
            <p className="text-sm">&copy; {currentYear} All rights reserved.</p>
          </div>
        </div>
      </div>
    </footer>
  );
}

### Hero section 
import { motion } from "framer-motion";
import { Button } from "@/components/ui/button";
import { ArrowRight } from "lucide-react";

export default function HeroSection() {
  const handleScrollTo = (sectionId: string) => {
    const element = document.getElementById(sectionId);
    if (element) {
      window.scrollTo({
        top: element.offsetTop - 64, // Header height
        behavior: "smooth",
      });
    }
  };

  return (
    <section 
      id="hero" 
      className="relative bg-gradient-to-br from-primary/5 to-background dark:from-gray-900 dark:to-gray-800 py-20 sm:py-28 md:py-32 overflow-hidden"
    >
      <div className="container mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
        <div className="max-w-3xl mx-auto text-center">
          <motion.h1 
            className="text-4xl sm:text-5xl md:text-6xl font-bold tracking-tight mb-6 bg-clip-text text-transparent bg-gradient-to-r from-primary/90 to-pink-500 dark:from-primary/80 dark:to-pink-500"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7 }}
          >
            Creative Developer Portfolio
          </motion.h1>
          
          <motion.p 
            className="text-lg sm:text-xl md:text-2xl text-muted-foreground mb-8"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7, delay: 0.2 }}
          >
            I build modern, responsive web applications with React
          </motion.p>
          
          <motion.div 
            className="flex flex-col sm:flex-row justify-center gap-4"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.7, delay: 0.4 }}
          >
            <Button 
              size="lg" 
              onClick={() => handleScrollTo("projects")}
              className="gap-2"
            >
              View My Work
              <ArrowRight className="h-4 w-4" />
            </Button>
            
            <Button 
              variant="outline" 
              size="lg" 
              onClick={() => handleScrollTo("contact")}
            >
              Get in Touch
            </Button>
          </motion.div>
        </div>
      </div>
      
      {/* Abstract background elements */}
      <div className="absolute top-0 left-0 w-full h-full overflow-hidden opacity-30 dark:opacity-20 pointer-events-none">
        <div className="absolute top-10 left-10 w-64 h-64 bg-primary/30 dark:bg-primary/50 rounded-full filter blur-3xl"></div>
        <div className="absolute bottom-10 right-10 w-80 h-80 bg-pink-500/20 rounded-full filter blur-3xl"></div>
      </div>
    </section>
  );
}

### projects section 
import { useSectionIntersect } from "@/hooks/use-section-intersect";
import { motion } from "framer-motion";
import { useState } from "react";
import { projects } from "@/data";
import { Button } from "@/components/ui/button";
import { ArrowRight } from "lucide-react";
import { Badge } from "@/components/ui/badge";
import { Eye, Github } from "lucide-react";

type ProjectCategory = "All" | "Web Apps" | "E-commerce" | "React";

export default function ProjectsSection() {
  const { ref, isInView } = useSectionIntersect();
  const [activeFilter, setActiveFilter] = useState<ProjectCategory>("All");

  const filteredProjects = activeFilter === "All" 
    ? projects 
    : projects.filter(project => project.category === activeFilter);

  return (
    <section 
      id="projects" 
      ref={ref} 
      className="py-20 bg-background"
    >
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        <motion.div 
          className="max-w-3xl mx-auto mb-12 text-center"
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.7 }}
        >
          <h2 className="text-3xl sm:text-4xl font-bold mb-4">Featured Projects</h2>
          <div className="h-1 w-20 bg-primary mx-auto mb-6"></div>
          <p className="text-lg text-muted-foreground">
            Explore my recent work and technical accomplishments
          </p>
        </motion.div>

        {/* Project Filters */}
        <motion.div 
          className="flex flex-wrap justify-center gap-2 mb-10"
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.7, delay: 0.2 }}
        >
          {["All", "Web Apps", "E-commerce", "React"].map((filter) => (
            <Button
              key={filter}
              variant={activeFilter === filter ? "default" : "ghost"}
              className="rounded-full text-sm"
              onClick={() => setActiveFilter(filter as ProjectCategory)}
            >
              {filter}
            </Button>
          ))}
        </motion.div>

        {/* Projects Grid */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {filteredProjects.map((project, index) => (
            <motion.div 
              key={project.id}
              className="group bg-background rounded-xl overflow-hidden shadow-md hover:shadow-xl border flex flex-col transition-all duration-300 hover:-translate-y-1"
              initial={{ opacity: 0, y: 20 }}
              animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
              transition={{ duration: 0.7, delay: 0.1 * index }}
            >
              <div className="relative overflow-hidden h-48">
                <div className="w-full h-full bg-muted flex items-center justify-center">
                  <svg 
                    className="w-1/3 h-1/3 text-muted-foreground/30" 
                    xmlns="http://www.w3.org/2000/svg" 
                    viewBox="0 0 24 24" 
                    fill="none" 
                    stroke="currentColor" 
                    strokeWidth="1" 
                    strokeLinecap="round" 
                    strokeLinejoin="round"
                  >
                    <rect width="18" height="18" x="3" y="3" rx="2" />
                    <path d="M3 9h18" />
                    <path d="M3 15h18" />
                    <path d="M9 3v18" />
                    <path d="M15 3v18" />
                  </svg>
                </div>
                <div className="absolute top-2 right-2">
                  <Badge variant="default" className="bg-primary text-primary-foreground">
                    {project.category}
                  </Badge>
                </div>
              </div>
              <div className="p-6 flex-grow">
                <h3 className="text-xl font-semibold mb-2">{project.title}</h3>
                <p className="text-muted-foreground mb-4">{project.description}</p>
                <div className="flex flex-wrap gap-2 mb-4">
                  {project.technologies.map(tech => (
                    <Badge key={tech} variant="outline" className="text-xs">
                      {tech}
                    </Badge>
                  ))}
                </div>
              </div>
              <div className="p-6 pt-0 mt-auto">
                <div className="flex space-x-4">
                  <Button variant="link" className="text-primary p-0 h-auto" asChild>
                    <a href={project.demoUrl} className="flex items-center">
                      <Eye className="mr-1 h-4 w-4" /> Demo
                    </a>
                  </Button>
                  <Button variant="link" className="text-primary p-0 h-auto" asChild>
                    <a href={project.codeUrl} className="flex items-center">
                      <Github className="mr-1 h-4 w-4" /> Code
                    </a>
                  </Button>
                </div>
              </div>
            </motion.div>
          ))}
        </div>

        <div className="text-center mt-12">
          <Button className="gap-2" size="lg">
            View All Projects
            <ArrowRight className="h-4 w-4" />
          </Button>
        </div>
      </div>
    </section>
  );
}

### skills section

import { useSectionIntersect } from "@/hooks/use-section-intersect";
import { motion } from "framer-motion";
import { useState, useEffect } from "react";
import { technicalSkills, professionalSkills } from "@/data";
import { 
  Code, 
  Wrench,
  Layout, 
  LayoutGrid, 
  GitBranch, 
  Users 
} from "lucide-react";
import { 
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";

// Helper function to render icon by name
const IconComponent = ({ iconName }: { iconName: string }) => {
  switch (iconName) {
    case "Layout":
      return <Layout className="text-2xl" />;
    case "LayoutGrid":
      return <LayoutGrid className="text-2xl" />;
    case "Workflow":
      return <Wrench className="text-2xl" />;
    case "TestTube":
      return <Code className="text-2xl" />;
    case "GitBranch":
      return <GitBranch className="text-2xl" />;
    case "Users":
      return <Users className="text-2xl" />;
    default:
      return <Code className="text-2xl" />;
  }
};

export default function SkillsSection() {
  const { ref, isInView } = useSectionIntersect();
  const [showSkills, setShowSkills] = useState(false);

  // Show skill bars with animation when section is in view
  useEffect(() => {
    if (isInView) {
      const timer = setTimeout(() => {
        setShowSkills(true);
      }, 300);
      
      return () => clearTimeout(timer);
    }
  }, [isInView]);

  return (
    <section 
      id="skills" 
      ref={ref} 
      className="py-20 bg-muted/50"
    >
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        <motion.div 
          className="max-w-3xl mx-auto mb-16 text-center"
          initial={{ opacity: 0, y: 20 }}
          animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.7 }}
        >
          <h2 className="text-3xl sm:text-4xl font-bold mb-4">Skills & Expertise</h2>
          <div className="h-1 w-20 bg-primary mx-auto mb-6"></div>
          <p className="text-lg text-muted-foreground">
            The technologies and tools I work with
          </p>
        </motion.div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-16">
          {/* Technical Skills */}
          <motion.div 
            className="space-y-6"
            initial={{ opacity: 0, x: -20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: -20 }}
            transition={{ duration: 0.7, delay: 0.2 }}
          >
            <h3 className="text-2xl font-semibold mb-6 text-primary flex items-center">
              <Code className="mr-2" /> Technical Skills
            </h3>
            
            {technicalSkills.map((skill, index) => (
              <div key={skill.name} className="space-y-2">
                <div className="flex justify-between items-center">
                  <span className="font-medium">{skill.name}</span>
                  <span className="text-sm text-muted-foreground">{skill.level}%</span>
                </div>
                <div className="w-full h-2 bg-muted rounded-full overflow-hidden">
                  <motion.div 
                    className="h-full bg-primary rounded-full"
                    initial={{ width: 0 }}
                    animate={{ width: showSkills ? `${skill.level}%` : 0 }}
                    transition={{ duration: 1, delay: 0.1 * index }}
                  />
                </div>
              </div>
            ))}
          </motion.div>

          {/* Professional Skills */}
          <motion.div
            initial={{ opacity: 0, x: 20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: 20 }}
            transition={{ duration: 0.7, delay: 0.4 }}
          >
            <h3 className="text-2xl font-semibold mb-6 text-primary flex items-center">
              <Wrench className="mr-2" /> Professional Skills
            </h3>

            <div className="grid grid-cols-2 sm:grid-cols-3 gap-4">
              {professionalSkills.map((skill, index) => (
                <motion.div 
                  key={skill.name}
                  className="bg-background rounded-lg p-5 border shadow-sm hover:shadow-md transition-shadow"
                  initial={{ opacity: 0, y: 20 }}
                  animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
                  transition={{ duration: 0.5, delay: 0.1 * index }}
                >
                  <div className="w-12 h-12 flex items-center justify-center bg-primary/10 text-primary rounded-lg mb-4">
                    <TooltipProvider>
                      <Tooltip>
                        <TooltipTrigger>
                          <IconComponent iconName={skill.iconName} />
                        </TooltipTrigger>
                        <TooltipContent>
                          <p>{skill.name}</p>
                        </TooltipContent>
                      </Tooltip>
                    </TooltipProvider>
                  </div>
                  <h4 className="font-semibold mb-1">{skill.name}</h4>
                  <p className="text-sm text-muted-foreground">{skill.description}</p>
                </motion.div>
              ))}
            </div>
          </motion.div>
        </div>
      </div>
    </section>
  );
}
