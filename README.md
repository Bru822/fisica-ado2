# Relatório: Simulação de Sistemas Físicos - Leis de Newton

## 1. Equações Fechadas e Derivação do Sistema
Para o plano inclinado com atrito e tração, isolamos os dois corpos recorrendo aos diagramas de corpo livre:

* **Corpo 1 (massa m1 sobre o plano com ângulo θ):** As forças atuantes ao longo do plano são a Tração (T) no sentido ascendente, a componente do peso paralela ao plano (m1 * g * sen(θ)) e a força de atrito (Fat) oposta ao movimento. Perpendicularmente ao plano, a força Normal equilibra a componente perpendicular do peso (N = m1 * g * cos(θ)).
**Equação da dinâmica (eixo do movimento):** T - m1 * g * sen(θ) - Fat = m1 * a

* **Corpo 2 (massa pendurada m2):** As forças atuantes no eixo vertical são o Peso do corpo 2 para baixo (m2 * g) e a Tração para cima (T).
**Equação da dinâmica:** m2 * g - T = m2 * a

Somando as duas equações para eliminar a variável interna da tração (T), obtemos: 
m2 * g - m1 * g * sen(θ) - Fat = (m1 + m2) * a

A "força motriz" intrínseca do sistema é Fmotriz = m2 * g - m1 * g * sen(θ). O atrito atua sempre opondo-se a esta força motriz, sendo a sua magnitude Fat = μk * N * sinal(Fmotriz). Rearranjando para a aceleração, deduzimos a equação fechada utilizada no código:
**a = [Fmotriz - μk * N * sinal(Fmotriz)] / (m1 + m2)**

---

## 2. Análise do Equilíbrio Estático
Consideremos as massas m1 = 5 kg, m2 = 3 kg num plano com θ = 30°. A força motriz é calculada por: 
Fmotriz = (3 * 9.81) - (5 * 9.81 * sen(30°)) = 29.43 - 24.525 = 4.905 N. 
A força Normal é N = 5 * 9.81 * cos(30°) ≈ 42.48 N.

A teoria estipula que o sistema permanece em repouso se |Fmotriz| ≤ μs * N. O valor crítico de atrito (ponto de iminência de movimento) será: 
μk = |Fmotriz| / N = 4.905 / 42.48 ≈ 0.115.

Ao executar a interface gráfica e ajustar os sliders para estes exatos valores, observa-se que, ao ultrapassar levemente o valor de 0.115 no coeficiente de atrito, a aceleração apresenta imediatamente o valor 0.00 m/s² (estado de repouso absoluto), confirmando a previsão teórica.

---

## 3. Comparação do Tempo para Atingir 95% da Velocidade Terminal (Sistema 3.3)
Para calcular o tempo que um corpo demora a atingir 95% da sua velocidade terminal (v_term), usamos as equações inversas da velocidade em ordem ao tempo.

* **Arrasto Linear:**
  * Equação: v(t) = v_term * (1 - e^(-t/τ))
  * Desenvolvimento: 0.95 * v_term = v_term * (1 - e^(-t/τ))
  * Simplificação: e^(-t/τ) = 0.05
  * Isolando o tempo: t_lin = -τ * ln(0.05) ≈ 2.99 * τ
  * Como a constante de tempo é τ = m/b, o tempo total é de aproximadamente **2.99 * (m/b) segundos**.

* **Arrasto Quadrático:**
  * Equação: v(t) = v_term * tanh((g * t) / v_term)
  * Desenvolvimento: 0.95 * v_term = v_term * tanh((g * t) / v_term)
  * Aplicando a função inversa: (g * t) / v_term = arctanh(0.95) ≈ 1.83
  * Isolando o tempo: t_quad = 1.83 * (v_term / g) = **1.83 * √(m / c*g) segundos**.

**Discussão da Diferença:** A forma das curvas é fundamentalmente diferente porque a dependência da velocidade varia. No arrasto linear (exponencial), a força de resistência cresce de modo estritamente proporcional à velocidade momentânea. No arrasto quadrático (tangente hiperbólica), a força de travagem escala com o quadrado da velocidade (v²). Isso significa que a força de arrasto aumenta de forma muito mais violenta e abrupta nos instantes em que o corpo atinge altas velocidades. Consequentemente, o modelo de arrasto quadrático "achata" a curva de aceleração mais cedo e converge para a assíntota de forma estruturalmente mais vigorosa do que o decaimento suave providenciado pela função exponencial.

---

## 4. Situações Reais de Aplicação
* **Sistema 3.1 (Plano Inclinado e Tração):** Tem aplicações diretas na engenharia de transportes, particularmente na conceção de guinchos, sistemas de tração por cabos na mineração a céu aberto, ou nos típicos funiculares europeus de montanha, onde uma carruagem (massa no plano inclinado) é contrabalançada por um peso suspenso num fosso ou por outra carruagem, minimizando o esforço mecânico exigido ao motor.
* **Sistema 3.3 (Força de Arrasto):** A simulação quadrática é o modelo padrão da cinemática para desenhar paraquedas e calcular o impacto de veículos aéreos não tripulados (drones) em caso de falha de motor aerodinâmica. O modelo de arrasto linear aplica-se sobretudo à microfísica, ilustrando o assentamento de pequenas poeiras atmosféricas ou a sedimentação de células sanguíneas num fluido espesso através da Lei de Stokes.
