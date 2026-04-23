#decyzja designowa

- nie mamy obrazków do reprezentowania zdarzenia (np. klasówki,kartkówki) -> używamy komponentu panelu 



Button design 

1. Dwa warianty - icon / no icon 
2. 3 stopnie hierarchii na każdy button 

https://www.figma.com/design/T4jvleMekvQX7jtT1xn0rP/Multitenant-materials?node-id=821-2513&t=7OADhkyGImCYrP3w-11 

Panel design:

https://www.figma.com/design/T4jvleMekvQX7jtT1xn0rP/Multitenant-materials?node-id=821-2537&t=4kQonyzxhIcTMAwJ-1

Subjects icons: 
Each subject will have it's own icon representing it 
- 🔢 Math
- ⌛ ️History
- 🧪 Chemistry 
- 🌍 Geography
- 🏴󠁧󠁢󠁥󠁮󠁧󠁿 English
- 🧬 Biology


## Panel description
Main info is the type of element. We will recognise 4 types of elements:
- exam 
- test
- oral exam 
- written assignment 

Statuses: 
Each element will have status basing on user action:
- To do 
- In progress 
- Done 
- After deadline  

Statuses are closely connected to actions available on panel. 
To do - action start 
In progress - action continue  
Done - action not available on panel
After deadline - status added automatically when element reached deadline and student did not marked it as done 


Students can mark elements as done in detailed view of element, available after clicking "Start" or Continue" on panel 

level of difficulty for an element is marked via chilli pepper
- 1 chilli pepper - easy 
- 2 chilli pepper - medium 
- 3 chilli pepper - hard

System based on what is actually used by teachers in schools, when creating assignment with different level of difficulty 

Main dashboard approved

https://www.figma.com/design/T4jvleMekvQX7jtT1xn0rP/Multitenant-materials?node-id=834-31846&t=iaa06oAvEZkchZDD-1
https://www.figma.com/design/T4jvleMekvQX7jtT1xn0rP/Multitenant-materials?node-id=834-9480&t=iaa06oAvEZkchZDD-1

1. Heading 1: Your upcoming events
2. Search (subject, description)
3. Multiselects: Subject, Event type, Status, Difficulty (can be used individually or combined)
4. Events displayed by dates
5. Events in done and after date status stay on the list
6. No weekend days
7. Empty state for days with no events