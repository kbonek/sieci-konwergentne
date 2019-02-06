# sieci-konwergentne
Menager dysków logicznych w systemach LINUX

Co to jest LVM (Logical Volume Manager)
Logical Volume Manager – mechanizm systemu operacyjnego do zarządzania przestrzenią pamięci masowej. System zaawansowanego zarządzania przestrzenią dyskową. Pozwala na połączenie partycji znajdujących się na różnych urządzeniach pamięci masowej w jeden dysk wirtualny, który jest o wiele bardziej elastyczny, niż klasyczne partycje dyskowe. Jego rozmiar nie jest zdefiniowany na stałe – jeśli zachodzi taka potrzeba, istnieje możliwość jego rozszerzenia o nową przestrzeń pamięciową. Obsługa woluminów logicznych zaimplementowana jest w większości systemów operacyjnych, może różnić się funkcjonalnością.

Struktur:
PV (physical volumes) - fizyczne woluminy: są bezpośrednio związane z partycjami dyskowymi (np. /dev/hda1, /dev/sdb3). ), które mogą być fizycznymi dyskami twardymi, partycjami lub numerami jednostek logicznych (LUN) na zewnętrznym urządzeniu przechowywania danych. Aby użyć urządzenia dla woluminu logicznego LVM, urządzenie musi zostać zainicjowane jako wolumin fizyczny (PV). Inicjowanie urządzenia blokowego jako woluminu fizycznego powoduje umieszczenie etykiety w pobliżu początku urządzenia. Woluminy fizyczne organizowane są w ciągi niewielkich bloków zwanych ekstentami fizycznymi.

VG (volume groups) - grupy woluminów: składają się z co najmniej jednego PV, ich wielkość to suma objętości wszystkich PV należących do danej grupy. Fizyczne ekstenty i grupy woluminów mają swoje logiczne odpowiedniki. W podstawowym scenariuszu są one mapowane jeden-do-jednego, tj. jednemu logicznemu ekstentowi odpowiada dokładnie jeden ekstent fizyczny. Możliwa jest jednak sytuacja, w której logiczny ekstent jest mapowany na kilka ekstentów fizycznych, z których każdy zawiera dokładnie te same informacje. Uzyskane miejsce dyskowe może być dowolnie dysponowane pomiędzy kolejne LV.

LV (logical volumes) - woluminy logiczne: są obszarami użytecznymi dla systemu (podobnie jak partycje dyskowe). LV są obszarami wydzielonymi z VG, zatem suma wielkości woluminów nie może przekraczać objętości VG, do którego należą. Logiczne ekstenty łączone są w logiczne woluminy (ang. logical volume), które są przez system operacyjny traktowane jako ciągłe bloki bajtów i mogą być używane dokładnie w taki sam sposób, jak partycje. System widzi wolumin jako ciągły obszar przestrzeni dyskowej, w rzeczywistości fizyczne ekstenty użyte do jego skonstruowania mogą być rozrzucone po całym fizycznym dysku lub też po kilku dyskach.

Instalacja PV, VG - sprawdzanie
rpm-qa-lvm sprawdzanie czy pakiet LVM jest dostępny

yum-install-lvm2 w przypadku braku pakietu, polecenie instalacyjne.

*pakiet LVM jest częścią podstawowa instalacji szanse na jego pominięcie są niewielkie. Zdarza się jednak, jeśli wybierzemy minimalną metodę instalacji pakietu, LVM nie zostanie zainstalowany. Trzeba zainstalować osobno pakiet LVM.

pvs-blank wyświetlanie bieżącego woluminu fizycznego.

#pvcreate polecenie umożliwi stworzenie objętości fizycznej (PV) z dysku twardego, tworzy nagłówek na każdej dostarczonej partycji.

Jeżeli jest jeden dysk twardy, tworzymy jeden PV na największej partycji. Gdy jest więcej dysków to na każdym tworzymy PV z partycji.

Do utworzenia partycji musisz ustawić typ na „Linux LVM”

Przy używaniu polecenia #pvcreate musimy się upewnić że celujemy w odpowiednie miejsce, polecenie to może spowodować utratę danych.

Fdisk-l- wyświetli listę bieżących partycji, punkt, i typ do ich montowania.

Do stworzenia woluminu fizycznego PV potrzebna jest jedna partycja oznaczona jako VLM Single-lvm-partiton

parted-last-partiton-end polecenie do zapisu końcowego punktu partycji.

parted-mkpart polecenie do stworzenia nowej partycji

parted-new-partition zweryfikowanie nowej partycji i jej numeru.

parted-print wyświetlanie bieżącej partycji

Komenda parted-quite-command wymusi aby jądra odczytało nową tablicę partycji.

pvdisplay by zweryfikować utworzone woluminy - śledzenia woluminów.

Można utworzyć grupę woluminów z pojedynczego woluminu i dodać do niej więcej woluminów za pomącą polecenia vgextend lub utworzyć grupę woluminów za pomącą komendy: #vgcreate VolGrup / dev / sad / dev /sad7 / dev / sdb1

Tworzenie logicznego woluminu odbywa się za pomocą polecenia: lvcreate

lvdisplay weryfikacja utworzonych woluminów

Podając nazwę nowego woluminu, jego rozmiar i grupę, na której będzie działała

#lvcreate –L [size] [volume_group] –n [logial_volume] pozwala utworzyć dwie partycję
