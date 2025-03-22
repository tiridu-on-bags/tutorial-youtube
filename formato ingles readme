# Modern Portfolio Website

A sleek, modern portfolio website built with React, featuring smooth animations, responsive design, and a clean UI.

![Portfolio Preview](https://via.placeholder.com/800x400)

## 📋 Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Component Structure](#component-structure)
  - [Hero Section](#hero-section)
  - [About Section](#about-section)
  - [Projects Section](#projects-section)
  - [Skills Section](#skills-section)
  - [Contact Section](#contact-section)
  - [Footer](#footer)
- [Installation](#installation)
- [Usage](#usage)
- [Customization](#customization)

## ✨ Features

- **Responsive Design**: Looks great on all device sizes
- **Smooth Animations**: Using Framer Motion for engaging transitions
- **Dark/Light Mode Support**: Compatible with system preferences
- **Form Validation**: Client-side validation for the contact form
- **Section Navigation**: Smooth scrolling between sections
- **Modular Components**: Easy to customize and extend

## 🛠️ Tech Stack

- **React**: Frontend library
- **Framer Motion**: Animation library
- **Tailwind CSS**: Utility-first CSS framework
- **shadcn/ui**: UI component library
- **React Hook Form**: Form handling
- **Zod**: Form validation
- **Lucide Icons**: Modern icon set

## 🧩 Component Structure

### Hero Section

The hero section creates a striking first impression with a gradient heading and animated introduction.

```jsx
export default function HeroSection() {
  return (
    <section id="hero" className="relative bg-gradient-to-br from-primary/5 to-background dark:from-gray-900 dark:to-gray-800 py-20 sm:py-28 md:py-32 overflow-hidden">
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
          
          {/* Rest of the hero section */}
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
```

### About Section

The about section uses a two-column layout to share professional background and personal information.

```jsx
export default function AboutSection() {
  const { ref, isInView } = useSectionIntersect();

  return (
    <section id="about" ref={ref} className="py-20 bg-background">
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
          {/* Who I Am column */}
          <motion.div 
            className="order-2 md:order-1"
            initial={{ opacity: 0, x: -20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: -20 }}
            transition={{ duration: 0.7, delay: 0.2 }}
          >
            <h3 className="text-2xl font-semibold mb-4 text-primary">Who I Am</h3>
            {/* Content continues... */}
          </motion.div>
          
          {/* Card column */}
          <motion.div 
            className="order-1 md:order-2 relative"
            initial={{ opacity: 0, x: 20 }}
            animate={isInView ? { opacity: 1, x: 0 } : { opacity: 0, x: 20 }}
            transition={{ duration: 0.7, delay: 0.4 }}
          >
            {/* Card content... */}
          </motion.div>
        </div>
      </div>
    </section>
  );
}
```

### Projects Section

The projects section showcases work with filterable categories and animated project cards.

```jsx
export default function ProjectsSection() {
  const { ref, isInView } = useSectionIntersect();
  const [activeFilter, setActiveFilter] = useState<ProjectCategory>("All");

  const filteredProjects = activeFilter === "All" 
    ? projects 
    : projects.filter(project => project.category === activeFilter);

  return (
    <section id="projects" ref={ref} className="py-20 bg-background">
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        {/* Section heading */}
        <motion.div className="max-w-3xl mx-auto mb-12 text-center">
          {/* Heading content... */}
        </motion.div>

        {/* Project Filters */}
        <motion.div className="flex flex-wrap justify-center gap-2 mb-10">
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
              {/* Project card content... */}
            </motion.div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

### Skills Section

The skills section displays technical abilities with animated progress bars and professional skills with icon cards.

```jsx
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
    <section id="skills" ref={ref} className="py-20 bg-muted/50">
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        {/* Section heading */}
        <motion.div className="max-w-3xl mx-auto mb-16 text-center">
          {/* Heading content... */}
        </motion.div>

        <div className="grid grid-cols-1 md:grid-cols-2 gap-16">
          {/* Technical Skills */}
          <motion.div className="space-y-6">
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
          <motion.div>
            {/* Professional skills content... */}
          </motion.div>
        </div>
      </div>
    </section>
  );
}
```

### Contact Section

The contact section provides a form and contact information with validation.

```jsx
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
    <section id="contact" ref={ref} className="py-20 bg-muted/50">
      <div className="container mx-auto px-4 sm:px-6 lg:px-8">
        {/* Section heading */}
        <motion.div className="max-w-3xl mx-auto mb-12 text-center">
          {/* Heading content... */}
        </motion.div>

        <div className="max-w-4xl mx-auto">
          <div className="grid grid-cols-1 md:grid-cols-5 gap-8">
            {/* Contact Info */}
            <motion.div className="md:col-span-2 space-y-6">
              {/* Contact cards... */}
            </motion.div>

            {/* Contact Form */}
            <motion.div className="md:col-span-3 bg-background p-6 rounded-lg shadow-sm border">
              <h3 className="text-xl font-semibold mb-6 text-primary">Send a Message</h3>
              
              <Form {...form}>
                <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
                  {/* Form fields... */}
                </form>
              </Form>
            </motion.div>
          </div>
        </div>
      </div>
    </section>
  );
}
```

### Footer

The footer includes social links and site information with dynamic year updates.

```jsx
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
              {/* Social links */}
            </div>
            <p className="text-sm">&copy; {currentYear} All rights reserved.</p>
          </div>
        </div>
      </div>
    </footer>
  );
}
```

## 🚀 Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/portfolio-website.git
cd portfolio-website
```

2. Install dependencies:
```bash
npm install
# or
yarn install
```

3. Start the development server:
```bash
npm run dev
# or
yarn dev
```

## 📝 Usage

The portfolio is structured into modular components that can be easily customized:

1. Update your personal information in the data files
2. Customize the color scheme in your Tailwind config
3. Add your own projects to the projects data file
4. Customize the skills section with your own expertise

## 🎨 Customization

### Changing Theme Colors

Edit the `tailwind.config.js` file to customize your color palette:

```js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#3B82F6', // Change this to your preferred primary color
          // Add additional shade variants if needed
        },
        // Add more custom colors as needed
      },
    },
  },
  // ...
};
```

### Adding Projects

Add your projects to the data file:

```js
// data.js or similar
export const projects = [
  {
    id: 1,
    title: "E-commerce Platform",
    description: "A full-featured online store with product listings, cart functionality, and checkout process.",
    category: "E-commerce",
    technologies: ["React", "Node.js", "MongoDB", "Stripe"],
    demoUrl: "https://demo-ecommerce.example.com",
    codeUrl: "https://github.com/yourusername/ecommerce-project",
  },
  // Add more projects...
];
```

### Updating Skills

Customize your technical and professional skills:

```js
// data.js or similar
export const technicalSkills = [
  { name: "React", level: 90 },
  { name: "JavaScript", level: 85 },
  { name: "HTML/CSS", level: 95 },
  { name: "Node.js", level: 75 },
  // Add more skills...
];

export const professionalSkills = [
  {
    name: "UI/UX Design",
    description: "Creating intuitive and engaging user interfaces",
    iconName: "Layout",
  },
  // Add more professional skills...
];
```

---

Feel free to reach out if you have any questions or need assistance with customization!
