##  Интерактивные материалы к отчету за 2023-2024 годы по проекту
### “Мультимасштабное моделирование нуклеосомной структуры хроматина как ключ к пониманию работы геномов эукариот.”

#### Задача 1.1. Построенные модели комплексов Sox2-нуклеосома для различных позиций
* [НУКЛ-Sox2<sub>pos19</sub>](constructed_complexes/sox2_pos_19_shift_0.pdb) - структура нуклеосомы в комплексе с Sox2 на позиции 19 (без сдвига нуклеосомной ДНК)
* [НУКЛ-Sox2<sub>pos20</sub>](constructed_complexes/sox2_pos_20_shift_1.pdb) - структура нуклеосомы в комплексе с Sox2 на позиции 20 (сдвиг нуклеосомной ДНК на 1 нуклеотид)
* [НУКЛ-Sox2<sub>pos39</sub>](constructed_complexes/sox2_pos_39_shift_0.pdb) - структура нуклеосомы в комплексе с Sox2 на позиции 39 (без сдвига нуклеосомной ДНК)
* [НУКЛ-Sox2<sub>pos40</sub>](constructed_complexes/sox2_pos_40_shift_1.pdb) - структура нуклеосомы в комплексе с Sox2 на позиции 40 (сдвиг нуклеосомной ДНК на 1 нуклеотид)
* [НУКЛ-Sox2<sub>pos50</sub>](constructed_complexes/sox2_pos_50_shift_1.pdb) - структура нуклеосомы в комплексе с Sox2 на позиции 50 (сдвиг нуклеосомной ДНК на 1 нуклеотид)

#### Задача 4.1. Анализ взаимодействий ПТФ с другими белками хроматина
* [Oct4 interactors](PTF_interactors/oct4_interactors.html) - белки-партнеры Oct4 по данным STRING, BioGRID и Gene Ontology
* [Sox2 interactors](PTF_interactors/sox2_interactors.html) - белки-партнеры Sox2 по данным STRING, BioGRID и Gene Ontology
* [Klf4 interactors](PTF_interactors/klf4_interactors.html) - белки-партнеры Klf4 по данным STRING, BioGRID и Gene Ontology
* [PDB](PTF_interactors/best_structures.zip) - предсказанные с помощью AlphaFold2 Multimer структуры комплексов пионерных транскрипционных факторов (Oct4, Sox2, Klf4) с белками-партнерами. Файлы PDB для комплексов с pTM > 0.5; 18, 24 и 6 комплексов для Oct4, Sox2 и Klf4 соответственно
  
#### В части молекулярного моделирования
#### Задача 5.1. Траектории молекулярной динамики нуклеосом в окружении разных моделей воды 
* [НУКЛ<sub>CAIPi3P</sub>](trajectories/ncp_caipi3p) - траектория МД нуклеосомы в окружении воды модели CAIPi3P в силовом поле Amber ff14SB + ParmBSC1 коррекции +Li/Merz ионы 
* [НУКЛ<sub>CAIPi3P, CUFIX</sub>](trajectories/ncp_caipi3p_cufix) - траектория МД нуклеосомы в окружении воды модели CAIPi3P в силовом поле Amber ff14SB + ParmBSC1 коррекции + CUFIX ионы 
* [НУКЛ<sub>TIP4P-D</sub>](trajectories/ncp_tip4pd) - траектория МД нуклеосомы в окружении воды модели TIP4P-D в силовом поле Amber ff14SB + ParmBSC1 коррекции +Li/Merz ионы 
* [НУКЛ<sub>TIP4P-D, CUFIX</sub>](trajectories/ncp_tip4pd_cufix) - траектория МД нуклеосомы в окружении воды модели TIP4P-D в силовом поле Amber ff14SB + ParmBSC1 коррекции + CUFIX ионы 
* [НУКЛ<sub>TIP4P-2005</sub>](trajectories/ncp_tip4p2005) - траектория МД нуклеосомы в окружении воды модели TIP4P-2005 в силовом поле Amber ff14SB + ParmBSC1 коррекции +Li/Merz ионы 
* [НУКЛ<sub>TIP4P-2005, CUFIX</sub>](trajectories/ncp_tip4p2005_cufix) - траектория МД нуклеосомы в окружении воды модели TIP4P-2005 в силовом поле Amber ff14SB + ParmBSC1 коррекции + CUFIX ионы
* [НУКЛ<sub>OPC</sub>](trajectories/ncp_opc) - траектория МД нуклеосомы в окружении воды модели OPC в силовом поле Amber ff14SB + ParmBSC1 коррекции + Li-Merz ионы 
* [НУКЛ<sub>TIP3P, CUFIX</sub>](trajectories/ncp_tip3p_opc) - траектория МД нуклеосомы в окружении воды модели TIP3P в силовом поле Amber ff14SB + ParmBSC1 коррекции + CUFIX ионы 

#### Задача 5.4. Траектории молекулярной динамики 
* [Тестовый расчет](trajectories/rbbna_test) управляемой МД нуклеосомы с применением внешних потенциалов к сайту посадки ремоделеров (SHL 2)
