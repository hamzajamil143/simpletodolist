├── README.md
├── index.html
├── script.js
└── style.css


/README.md:
--------------------------------------------------------------------------------
1 | # simpletodolist


--------------------------------------------------------------------------------
/index.html:
--------------------------------------------------------------------------------
 1 | <!DOCTYPE html>
 2 | <html lang="en">
 3 |   <head>
 4 |     <meta charset="UTF-8" />
 5 |     <meta name="viewport" content="width=device-width, initial-scale=1.0" />
 6 |     <title>TodoList</title>
 7 |     <link rel="stylesheet" href="style.css">
 8 |   </head>
 9 |   <body>
10 |     <h1>Simple Todo List</h1>
11 |     <p>Welcome to this todo list. Double link to edit the todo.</p>
12 | 
13 |     <input type="text" id="todo-input" placeholder="Enter a todo" />
14 | 
15 |     <button id="add-btn">Add</button>
16 | 
17 |     <ul id="todo-list"></ul>
18 | 
19 |     <script src="script.js"></script>
20 |   </body>
21 | </html>
22 | 


--------------------------------------------------------------------------------
/script.js:
--------------------------------------------------------------------------------
 1 | const input = document.getElementById("todo-input");
 2 | const add = document.getElementById("add-btn");
 3 | const list = document.getElementById("todo-list");
 4 | 
 5 | const saved = localStorage.getItem("todos");
 6 | const todos = saved ? JSON.parse(saved) : [];
 7 | 
 8 | function saveTodos() {
 9 |   // save array to local S
10 |   localStorage.setItem("todos", JSON.stringify(todos));
11 | }
12 | 
13 | function createTodoNode(todo, index) {
14 |   const li = document.createElement("li");
15 | 
16 |   const checkbox = document.createElement("input");
17 |   checkbox.type = "checkbox";
18 |   checkbox.checked = !!todo.completed;
19 |   checkbox.addEventListener("change", () => {
20 |     todo.completed = checkbox.checked;
21 | 
22 |     textSpan.style.textDecoration = todo.completed ? "line-through" : "";
23 |     saveTodos();
24 |   });
25 | 
26 |   const textSpan = document.createElement("span");
27 |   textSpan.textContent = todo.text;
28 |   textSpan.style.margin = "0 8px";
29 |   if (todo.completed) {
30 |     textSpan.style.textDecoration = "line-through";
31 |   }
32 | 
33 |   textSpan.addEventListener("dblclick", () => {
34 |     const newText = prompt("Edit todo");
35 |     if (newText !== null) {
36 |       todo.text = newText.trim();
37 |       textSpan.textContent = todo.text;
38 |       saveTodos();
39 |     }
40 |   });
41 | 
42 |   const delBtn = document.createElement("button");
43 |   delBtn.textContent = "Delete";
44 |   delBtn.addEventListener("click", () => {
45 |     todos.splice(index, 1);
46 |     render();
47 |     saveTodos();
48 |   });
49 | 
50 |   li.appendChild(checkbox);
51 |   li.appendChild(textSpan);
52 |   li.appendChild(delBtn);
53 |   return li;
54 | }
55 | 
56 | function render() {
57 |   list.innerHTML = "";
58 | 
59 |   todos.forEach((todo, index) => {
60 |     const node = createTodoNode(todo, index);
61 |     list.appendChild(node);
62 |   });
63 | }
64 | 
65 | function addTodo() {
66 |   const text = input.value.trim();
67 |   if (!text) {
68 |     return;
69 |   }
70 | 
71 |   todos.push({ text, completed: false });
72 |   input.value = "";
73 |   render();
74 |   saveTodos();
75 | }
76 | 
77 | add.addEventListener("click", addTodo);
78 | input.addEventListener("keydown", (e) => {
79 |   if (e.key == "Enter") {
80 |     addTodo();
81 |   }
82 | });
83 | render();
84 | 


--------------------------------------------------------------------------------
/style.css:
--------------------------------------------------------------------------------
  1 | /* -------------------- GLOBAL RESET -------------------- */
  2 | * {
  3 |   margin: 0;
  4 |   padding: 0;
  5 |   box-sizing: border-box;
  6 |   font-family: "Poppins", sans-serif;
  7 | }
  8 | 
  9 | body {
 10 |   background: linear-gradient(135deg, #6a11cb, #2575fc);
 11 |   min-height: 100vh;
 12 |   display: flex;
 13 |   flex-direction: column;
 14 |   align-items: center;
 15 |   justify-content: start;
 16 |   padding-top: 80px;
 17 |   color: #fff;
 18 |   overflow-x: hidden;
 19 | }
 20 | 
 21 | /* -------------------- HEADER -------------------- */
 22 | h1 {
 23 |   font-size: 2.75rem;
 24 |   margin-bottom: 8px;
 25 |   letter-spacing: 1px;
 26 |   background: linear-gradient(90deg, #ffffff, #dbeafe);
 27 |   -webkit-background-clip: text;
 28 |   -webkit-text-fill-color: transparent;
 29 |   text-align: center;
 30 | }
 31 | 
 32 | p {
 33 |   color: #e0e7ff;
 34 |   margin-bottom: 25px;
 35 |   font-size: 1rem;
 36 |   text-align: center;
 37 | }
 38 | 
 39 | /* -------------------- INPUT AREA -------------------- */
 40 | .input-container {
 41 |   display: flex;
 42 |   align-items: center;
 43 |   justify-content: center;
 44 |   gap: 12px;
 45 |   flex-wrap: wrap;
 46 | }
 47 | 
 48 | #todo-input {
 49 |   width: 320px;
 50 |   padding: 12px 16px;
 51 |   border-radius: 12px;
 52 |   border: 1px solid rgba(255, 255, 255, 0.3);
 53 |   background: rgba(255, 255, 255, 0.15);
 54 |   color: #fff;
 55 |   outline: none;
 56 |   font-size: 1rem;
 57 |   transition: all 0.35s ease;
 58 |   backdrop-filter: blur(12px);
 59 | }
 60 | 
 61 | #todo-input::placeholder {
 62 |   color: rgba(255, 255, 255, 0.6);
 63 | }
 64 | 
 65 | #todo-input:focus {
 66 |   border-color: #5eead4;
 67 |   box-shadow: 0 0 12px rgba(94, 234, 212, 0.6);
 68 |   transform: scale(1.03);
 69 | }
 70 | 
 71 | /* -------------------- ADD BUTTON -------------------- */
 72 | #add-btn {
 73 |   padding: 12px 20px;
 74 |   margin: 20px 0;
 75 |   background: linear-gradient(90deg, #6366f1, #8b5cf6);
 76 |   color: #fff;
 77 |   border: none;
 78 |   border-radius: 12px;
 79 |   cursor: pointer;
 80 |   font-weight: 600;
 81 |   transition: all 0.35s ease;
 82 |   box-shadow: 0 6px 18px rgba(99, 102, 241, 0.4);
 83 | }
 84 | 
 85 | #add-btn:hover {
 86 |   background: linear-gradient(90deg, #818cf8, #a78bfa);
 87 |   transform: translateY(-3px);
 88 |   box-shadow: 0 8px 22px rgba(129, 140, 248, 0.5);
 89 | }
 90 | 
 91 | /* -------------------- TODO LIST -------------------- */
 92 | #todo-list {
 93 |   list-style: none;
 94 |   margin-top: 35px;
 95 |   margin-bottom: 35px;
 96 |   padding: 0;
 97 |   width: 380px;
 98 |   background: rgba(255, 255, 255, 0.12);
 99 |   border-radius: 18px;
100 |   backdrop-filter: blur(20px);
101 |   box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
102 |   overflow: hidden;
103 | }
104 | 
105 | /* -------------------- TODO ITEM -------------------- */
106 | #todo-list li {
107 |   display: flex;
108 |   align-items: center;
109 |   justify-content: space-between;
110 |   padding: 14px 18px;
111 |   color: #f8fafc;
112 |   border-bottom: 1px solid rgba(255, 255, 255, 0.1);
113 |   animation: fadeIn 0.4s ease forwards;
114 |   transition: all 0.3s ease;
115 | }
116 | 
117 | #todo-list li:hover {
118 |   background: rgba(255, 255, 255, 0.08);
119 | }
120 | 
121 | /* -------------------- CHECKBOX -------------------- */
122 | #todo-list input[type="checkbox"] {
123 |   accent-color: #5eead4;
124 |   transform: scale(1.3);
125 |   margin-right: 10px;
126 |   cursor: pointer;
127 | }
128 | 
129 | /* -------------------- TODO TEXT -------------------- */
130 | #todo-list span {
131 |   flex: 1;
132 |   cursor: pointer;
133 |   font-size: 1rem;
134 |   transition: color 0.3s ease;
135 | }
136 | 
137 | #todo-list span:hover {
138 |   color: #a5f3fc;
139 | }
140 | 
141 | #todo-list li input[type="checkbox"]:checked + span {
142 |   text-decoration: line-through;
143 |   color: #cbd5e1;
144 |   transition: all 0.3s ease;
145 | }
146 | 
147 | /* -------------------- DELETE BUTTON -------------------- */
148 | #todo-list button {
149 |   background: linear-gradient(90deg, #fb7185, #f43f5e);
150 |   color: #fff;
151 |   border: none;
152 |   border-radius: 8px;
153 |   padding: 6px 12px;
154 |   cursor: pointer;
155 |   font-size: 0.9rem;
156 |   font-weight: 500;
157 |   transition: all 0.3s ease;
158 |   box-shadow: 0 3px 8px rgba(251, 113, 133, 0.4);
159 | }
160 | 
161 | #todo-list button:hover {
162 |   background: linear-gradient(90deg, #f87171, #ef4444);
163 |   transform: scale(1.08);
164 | }
165 | 
166 | /* -------------------- ANIMATIONS -------------------- */
167 | @keyframes fadeIn {
168 |   from {
169 |     opacity: 0;
170 |     transform: translateY(10px);
171 |   }
172 |   to {
173 |     opacity: 1;
174 |     transform: translateY(0);
175 |   }
176 | }
177 | 
178 | /* -------------------- RESPONSIVE -------------------- */
179 | @media (max-width: 480px) {
180 |   body {
181 |     padding-top: 50px;
182 |   }
183 |   #todo-input {
184 |     width: 220px;
185 |   }
186 |   #todo-list {
187 |     width: 320px;
188 |   }
189 |   #add-btn {
190 |     padding: 10px 16px;
191 |   }
192 | }
193 | 


--------------------------------------------------------------------------------
