%çizgiclc; clear; close all;

% Parametreler
m = 1.0;        % kütle (kg)
g = 9.81;       % yerçekimi (m/s^2)
T = 12;         % thrust (N)
dt = 0.01;      % zaman adımı
t_final = 5;    % toplam süre


% Başlangıç koşulları
h1 = 0; v1 = 0; % explicit Euler
h2 = 0; v2 = 0; % semi-implicit Euler

time = 0:dt:t_final;
H1 = zeros(size(time)); % ...elemanlık sıfırlardan oluşan boş dizi yaratıyor
H2 = zeros(size(time));

for i = 1:length(time) % time vektörünün uzunluğu kadar döngü çalıştır demek
    % İvme
    a = (T - m*g)/m;
    
    % --- Explicit Euler ---
    v1_new = v1 + a*dt;
    h1_new = h1 + v1_new*dt; % eski hızı kullandık
    v1 = v1_new;              % bir sonraki adım için hız güncelle
    h1 = h1_new; 
    % --- Semi-implicit Euler ---
    v2_new = v2 + a*dt;
    h2_new = h2 + v2_new*dt;
    v2 = v2_new;              % bir sonraki adım için hız güncelle
    h2 = h2_new; 
  % yeni hızı kullandık
    
    H1(i) = h1; % her zaman adımında hesaplanan yükseklikleri bu boş dizilerin içine koyuyoruz
    H2(i) = h2; %H2 dizisinin i'inci kutusuna o anki yüksekliği koy" demek
end

% Grafik
figure; % Yeni bir grafik penceresi açıyor.
plot(time, H1, 'b', 'LineWidth', 2); hold on; % x ekseni (zaman)H1 → y ekseni (yükseklik)'b' → çizgi rengi mavi (blue) `LineWidth', 2 → çizgi kalınlığı 2
plot(time, H2, 'r--', 'LineWidth', 2);
xlabel('Zaman (s)'); ylabel('Yükseklik (m)'); % x ve y eksenlerini etiketliyor, yani grafikte "zaman" ve "yükseklik" yazıyor
title('Euler yöntemleri karşılaştırma');
legend('Explicit Euler', 'Semi-implicit Euler'); lerin adını verir
