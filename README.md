# sport-upcode
import { useEffect, useState } from "react";
import { useNavigate } from "react-router-dom";
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";
import { MapPin, Users, Calendar, Zap, LogOut } from "lucide-react";
import { supabase } from "@/integrations/supabase/client";
import { useToast } from "@/hooks/use-toast";

const Index = () => {
  const navigate = useNavigate();
  const { toast } = useToast();
  const [user, setUser] = useState<any>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
   
    // Check authentication status
    
    supabase.auth.getSession().then(({ data: { session } }) => {
      
      setUser(session?.user ?? null);
    
      setLoading(false);
    
    })

    const { data: { subscription } } = supabase.auth.onAuthStateChange((_event, session) => {
      setUser(session?.user ?? null);
    });

    return () => subscription.unsubscribe();
  }, []);

  const handleLogout = async () => {
    try {
      await supabase.auth.signOut();
      toast({
        title: "Logout realizado",
        description: "Até logo!",
      });
    } catch (error: any) {
      toast({
        title: "Erro ao fazer logout",
        description: error.message,
        variant: "destructive",
      });
    }
  };

  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center smoke-bg">
        <div className="animate-spin rounded-full h-12 w-12 border-t-2 border-b-2 border-primary"></div>
      </div>
    );
  }

  return (
    
    <div className="min-h-screen bg-background">
      
      {/* Hero Section */}
      
      section className="relative min-h-screen flex items-center justify-center overflow-hidden smoke-bg"
        
        div className="absolute inset-0 bg-gradient-to-b from-primary/20 via-transparent to-background" /
        
        {user && (
          <Button
            variant="outline"
            size="sm"
            onClick={handleLogout}
            className="absolute top-6 right-6 z-10"
          >
            <LogOut className="w-4 h-4 mr-2" />
            Sair
          </Button>
        )}

        <div className="container mx-auto px-4 relative z-10">
          <div className="max-w-4xl mx-auto text-center space-y-8 animate-fade-in-up">
            {/* Logo/Brand */}
            <div className="inline-flex items-center gap-3 px-6 py-3 rounded-full bg-primary/10 border border-primary/30 glow-border">
              <Zap className="w-6 h-6 text-accent" />
              <span className="text-lg font-semibold text-accent">Sport-Up</span>
            </div>

            {/* Main Headline */}
            <h1 className="text-5xl md:text-7xl font-bold tracking-tight glow-text">
              Encontre Parceiros
              <br />
              <span className="text-accent">Para o Seu Esporte</span>
            </h1>

            <p className="text-xl md:text-2xl text-muted-foreground max-w-2xl mx-auto">
              A rede social esportiva de Brasília. Descubra quadras, organize partidas e conecte-se com atletas da sua região.
            </p>

            {/* CTA Buttons */}
            <div className="flex flex-col sm:flex-row gap-4 justify-center pt-4">
              {user ? (
                <>
                  <Button 
                    size="lg" 
                    className="text-lg px-8 py-6 bg-accent hover:bg-accent/90 text-accent-foreground font-semibold shadow-lg hover:shadow-accent/50 transition-all"
                    onClick={() => navigate("/map")}
                  >
                    Explorar Quadras
                  </Button>
                  <Button 
                    size="lg" 
                    variant="outline" 
                    className="text-lg px-8 py-6 border-accent/30 hover:bg-accent/10 hover:border-accent"
                    onClick={() => navigate("/profile")}
                  >
                    Ver Perfil
                  </Button>
                </>
              ) : (
                <>
                  <Button 
                    size="lg" 
                    className="text-lg px-8 py-6 bg-accent hover:bg-accent/90 text-accent-foreground font-semibold shadow-lg hover:shadow-accent/50 transition-all"
                    onClick={() => navigate("/auth")}
                  >
                    Começar Agora
                  </Button>
                  <Button 
                    size="lg" 
                    variant="outline" 
                    className="text-lg px-8 py-6 border-accent/30 hover:bg-accent/10 hover:border-accent"
                    onClick={() => navigate("/map")}
                  >
                    Explorar Quadras
                  </Button>
                </>
              )}
            </div>
          </div>
        </div>

        {/* Floating Elements */}
        <div className="absolute inset-0 pointer-events-none">
          <div className="absolute top-1/4 left-1/4 w-64 h-64 bg-accent/5 rounded-full blur-3xl animate-smoke-float" />
          <div className="absolute bottom-1/4 right-1/4 w-96 h-96 bg-primary/10 rounded-full blur-3xl animate-smoke-float" style={{ animationDelay: '5s' }} />
        </div>
      </section>

      {/* Features Section */}
      <section className="py-24 px-4 relative">
        <div className="container mx-auto">
          <div className="text-center mb-16 space-y-4">
            <h2 className="text-4xl md:text-5xl font-bold">Como Funciona</h2>
            <p className="text-xl text-muted-foreground">Três passos simples para começar a jogar</p>
          </div>

          <div className="grid md:grid-cols-3 gap-8 max-w-6xl mx-auto">
            {features.map((feature, index) => (
              <Card 
                key={feature.title}
                className="p-8 bg-card/50 backdrop-blur-sm border-border/50 hover:border-accent/50 transition-all duration-300 hover:shadow-lg hover:shadow-accent/10 group animate-fade-in"
                style={{ animationDelay: `${index * 0.2}s` }}
              >
                <div className="mb-6 inline-flex p-4 rounded-2xl bg-accent/10 group-hover:bg-accent/20 transition-colors">
                  <feature.icon className="w-8 h-8 text-accent" />
                </div>
                <h3 className="text-2xl font-bold mb-3 group-hover:text-accent transition-colors">{feature.title}</h3>
                <p className="text-muted-foreground leading-relaxed">{feature.description}</p>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Stats Section */}
      <section className="py-24 px-4 bg-primary/5">
        <div className="container mx-auto">
          <div className="grid grid-cols-2 md:grid-cols-4 gap-8 max-w-4xl mx-auto">
            {stats.map((stat) => (
              <div key={stat.label} className="text-center space-y-2">
                <div className="text-4xl md:text-5xl font-bold text-accent glow-text">{stat.value}</div>
                <div className="text-sm md:text-base text-muted-foreground">{stat.label}</div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Sports Section */}
      <section className="py-24 px-4">
        <div className="container mx-auto">
          <div className="text-center mb-16 space-y-4">
            <h2 className="text-4xl md:text-5xl font-bold">Esportes Disponíveis</h2>
            <p className="text-xl text-muted-foreground">Encontre partidas do seu esporte favorito</p>
          </div>

          <div className="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4 max-w-6xl mx-auto">
            {sports.map((sport, index) => (
              <div 
                key={sport}
                className="p-6 rounded-2xl bg-card/50 backdrop-blur-sm border border-border/50 hover:border-accent/50 text-center font-semibold hover:bg-accent/10 transition-all duration-300 cursor-pointer animate-scale-in"
                style={{ animationDelay: `${index * 0.1}s` }}
              >
                {sport}
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* CTA Section */}
      <section className="py-24 px-4 relative overflow-hidden">
        <div className="absolute inset-0 bg-gradient-to-t from-primary/20 to-transparent" />
        <div className="container mx-auto relative z-10">
          <div className="max-w-3xl mx-auto text-center space-y-8">
            <h2 className="text-4xl md:text-5xl font-bold glow-text">
              Pronto Para Começar?
            </h2>
            <p className="text-xl text-muted-foreground">
              Junte-se a milhares de atletas em Brasília e encontre seu próximo jogo hoje mesmo.
            </p>
            {!user && (
              <Button 
                size="lg" 
                className="text-lg px-12 py-6 bg-accent hover:bg-accent/90 text-accent-foreground font-semibold shadow-xl hover:shadow-accent/50 transition-all animate-glow-pulse"
                onClick={() => navigate("/auth")}
              >
                Criar Conta Grátis
              </Button>
            )}
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="py-12 px-4 border-t border-border/30">
        <div className="container mx-auto">
          <div className="flex flex-col md:flex-row justify-between items-center gap-6">
            <div className="flex items-center gap-2">
              <Zap className="w-6 h-6 text-accent" />
              <span className="text-lg font-semibold">Sport-Up</span>
            </div>
            <div className="text-sm text-muted-foreground">
              © 2025 Sport-Up. Conectando atletas em Brasília.
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
};

const features = [
  {
    icon: MapPin,
    title: "Encontre Quadras",
    description: "Veja todas as quadras públicas e privadas de Brasília em um mapa interativo com disponibilidade em tempo real."
  },
  {
    icon: Calendar,
    title: "Organize Partidas",
    description: "Crie jogos, defina horário, nível e vagas. Gerencie tudo em um só lugar de forma simples e rápida."
  },
  {
    icon: Users,
    title: "Conecte-se",
    description: "Encontre parceiros do seu nível, converse por chat e forme seu time ideal para qualquer esporte."
  }
];

const stats = [
  { value: "150+", label: "Quadras Mapeadas" },
  { value: "2.5K+", label: "Atletas Ativos" },
  { value: "500+", label: "Partidas/Semana" },
  { value: "20", label: "Esportes" }
];

const sports = [
  "⚽ Futebol",
  "🏐 Vôlei", 
  "🏀 Basquete",
  "🎾 Tênis",
  "🏃 Corrida",
  "⚾ Futevôlei",
  "🤾 Handebol",
  "🏸 Badminton",
  "🥋 Artes Marciais",
  "🏊 Natação",
  "🚴 Ciclismo",
  "⛹️ Crossfit"
];

export default Index;
