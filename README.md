<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Domowy Asystent Budżetowy - zarządzaj wydatkami, celami i planuj posiłki">
    <meta name="theme-color" content="#4F46E5">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="apple-mobile-web-app-title" content="Budżet Pro">
    <title>Asystent Budżetowy Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiQXN5c3RlbnQgQnVkxbxldG93eSIsInNob3J0X25hbWUiOiJCdWTFvGV0Iiwic3RhcnRfdXJsIjoiLiIsImRpc3BsYXkiOiJzdGFuZGFsb25lIiwiYmFja2dyb3VuZF9jb2xvciI6IiNmZmZmZmYiLCJ0aGVtZV9jb2xvciI6IiM0RjQ2RTUiLCJpY29ucyI6W3sic3JjIjoiZGF0YTppbWFnZS9zdmcreG1sLCUzQ3N2ZyB4bWxucz0naHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnIHdpZHRoPScxOTInIGhlaWdodD0nMTkyJyB2aWV3Qm94PScwIDAgMTkyIDE5Mic+JTNDcmVjdCB3aWR0aD0nMTkyJyBoZWlnaHQ9JzE5MicgZmlsbD0nJTIzNEY0NkU1Jy8+JTNDdGV4dCB4PSc1MCUyNScgeT0nNTAlMjUnIGZvbnQtc2l6ZT0nODAnIGZpbGw9J3doaXRlJyB0ZXh0LWFuY2hvcj0nbWlkZGxlJyBkeT0nLjM1ZW0nPiQlM0MvdGV4dD4lM0Mvc3ZnPiIsInNpemVzIjoiMTkyeDE5MiIsInR5cGUiOiJpbWFnZS9zdmcreG1sIn1dfQ==">
    <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>💰</text></svg>">
    <style>
        body { overscroll-behavior: none; }
        .animate-pulse-slow { animation: pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100">
    <div id="app"></div>

    <script>
        // Ikony SVG jako stringi
        const icons = {
            home: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m3 9 9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>',
            plus: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>',
            target: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><circle cx="12" cy="12" r="6"/><circle cx="12" cy="12" r="2"/></svg>',
            utensils: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 2v7c0 1.1.9 2 2 2h4a2 2 0 0 0 2-2V2"/><path d="M7 2v20"/><path d="M21 15V2v0a5 5 0 0 0-5 5v6c0 1.1.9 2 2 2h3Zm0 0v7"/></svg>',
            settings: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12.22 2h-.44a2 2 0 0 0-2 2v.18a2 2 0 0 1-1 1.73l-.43.25a2 2 0 0 1-2 0l-.15-.08a2 2 0 0 0-2.73.73l-.22.38a2 2 0 0 0 .73 2.73l.15.1a2 2 0 0 1 1 1.72v.51a2 2 0 0 1-1 1.74l-.15.09a2 2 0 0 0-.73 2.73l.22.38a2 2 0 0 0 2.73.73l.15-.08a2 2 0 0 1 2 0l.43.25a2 2 0 0 1 1 1.73V20a2 2 0 0 0 2 2h.44a2 2 0 0 0 2-2v-.18a2 2 0 0 1 1-1.73l.43-.25a2 2 0 0 1 2 0l.15.08a2 2 0 0 0 2.73-.73l.22-.39a2 2 0 0 0-.73-2.73l-.15-.08a2 2 0 0 1-1-1.74v-.5a2 2 0 0 1 1-1.74l.15-.09a2 2 0 0 0 .73-2.73l-.22-.38a2 2 0 0 0-2.73-.73l-.15.08a2 2 0 0 1-2 0l-.43-.25a2 2 0 0 1-1-1.73V4a2 2 0 0 0-2-2z"/><circle cx="12" cy="12" r="3"/></svg>',
            users: '<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M22 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>',
            dollar: '<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>',
            chart: '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="20" x2="18" y2="10"/><line x1="12" y1="20" x2="12" y2="4"/><line x1="6" y1="20" x2="6" y2="14"/></svg>',
            sparkles: '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="m12 3-1.912 5.813a2 2 0 0 1-1.275 1.275L3 12l5.813 1.912a2 2 0 0 1 1.275 1.275L12 21l1.912-5.813a2 2 0 0 1 1.275-1.275L21 12l-5.813-1.912a2 2 0 0 1-1.275-1.275L12 3Z"/><path d="M5 3v4"/><path d="M19 17v4"/><path d="M3 5h4"/><path d="M17 19h4"/></svg>',
            bell: '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M6 8a6 6 0 0 1 12 0c0 7 3 9 3 9H3s3-2 3-9"/><path d="M10.3 21a1.94 1.94 0 0 0 3.4 0"/></svg>',
            trash: '<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 6h18"/><path d="M19 6v14c0 1-1 2-2 2H7c-1 0-2-1-2-2V6"/><path d="M8 6V4c0-1 1-2 2-2h4c1 0 2 1 2 2v2"/></svg>'
        };

        // Stan aplikacji
        let state = {
            activeTab: 'home',
            currentUser: '',
            data: {
                users: [],
                expenses: [],
                mealPlans: [],
                goals: [],
                household: { dailyBudget: 60, preferences: '', allergies: '' },
                notifications: { dailyReminder: true, budgetAlert: true, goalProgress: true }
            },
            newExpense: { date: new Date().toISOString().split('T')[0], category: 'Jedzenie', name: '', amount: '' },
            newGoal: { name: '', target: '', deadline: '' },
            aiResponse: '',
            isLoadingAI: false
        };

        const categories = ['Jedzenie', 'Używki', 'Rachunki', 'Transport', 'Rozrywka', 'Inne'];

        // Załaduj dane
        function loadData() {
            const saved = localStorage.getItem('budgetAppData');
            if (saved) {
                try {
                    state.data = JSON.parse(saved);
                    if (state.data.users.length > 0 && !state.currentUser) {
                        state.currentUser = state.data.users[0];
                    }
                } catch (e) {
                    console.error('Error loading data:', e);
                }
            }
        }

        // Zapisz dane
        function saveData() {
            localStorage.setItem('budgetAppData', JSON.stringify(state.data));
        }

        // Dodaj użytkownika
        function addUser(name) {
            if (name && !state.data.users.includes(name)) {
                state.data.users.push(name);
                if (!state.currentUser) state.currentUser = name;
                saveData();
                render();
            }
        }

        // Dodaj wydatek
        function addExpense() {
            if (!state.newExpense.name || !state.newExpense.amount || !state.currentUser) return;
            
            const expense = {
                ...state.newExpense,
                amount: parseFloat(state.newExpense.amount),
                user: state.currentUser,
                id: Date.now()
            };
            
            state.data.expenses.push(expense);
            state.newExpense = { 
                date: new Date().toISOString().split('T')[0], 
                category: 'Jedzenie', 
                name: '', 
                amount: '' 
            };
            
            checkBudgetAlert(expense.amount);
            saveData();
            render();
        }

        // Dodaj cel
        function addGoal() {
            if (!state.newGoal.name || !state.newGoal.target) return;
            
            const goal = {
                ...state.newGoal,
                target: parseFloat(state.newGoal.target),
                saved: 0,
                id: Date.now()
            };
            
            state.data.goals.push(goal);
            state.newGoal = { name: '', target: '', deadline: '' };
            saveData();
            render();
        }

        // Aktualizuj postęp celu
        function updateGoalProgress(goalId, amount) {
            const goal = state.data.goals.find(g => g.id === goalId);
            if (goal) {
                goal.saved += parseFloat(amount);
                saveData();
                render();
            }
        }

        // Alert budżetowy
        function checkBudgetAlert(newAmount) {
            if (!state.data.notifications.budgetAlert) return;
            
            const today = new Date().toISOString().split('T')[0];
            const todayExpenses = state.data.expenses
                .filter(e => e.date === today && e.user === state.currentUser)
                .reduce((sum, e) => sum + e.amount, 0) + newAmount;
            
            if (todayExpenses > state.data.household.dailyBudget) {
                alert(`⚠️ Przekroczyłeś dzienny budżet!\\n${todayExpenses.toFixed(2)} zł / ${state.data.household.dailyBudget} zł`);
            }
        }

        // Oblicz statystyki
        function calculateStats() {
            const userExpenses = state.data.expenses.filter(e => e.user === state.currentUser);
            const today = new Date().toISOString().split('T')[0];
            const weekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];
            const monthStart = new Date(new Date().getFullYear(), new Date().getMonth(), 1).toISOString().split('T')[0];
            
            const todayTotal = userExpenses.filter(e => e.date === today).reduce((sum, e) => sum + e.amount, 0);
            const weekTotal = userExpenses.filter(e => e.date >= weekAgo).reduce((sum, e) => sum + e.amount, 0);
            const monthTotal = userExpenses.filter(e => e.date >= monthStart).reduce((sum, e) => sum + e.amount, 0);
            
            const byCategory = {};
            categories.forEach(cat => {
                byCategory[cat] = userExpenses
                    .filter(e => e.category === cat && e.date >= monthStart)
                    .reduce((sum, e) => sum + e.amount, 0);
            });
            
            return { todayTotal, weekTotal, monthTotal, byCategory };
        }

        // Generator AI
        async function generateAIMealPlan() {
            state.isLoadingAI = true;
            state.aiResponse = '';
            render();
            
            const prompt = `Jesteś dietetykiem planującym posiłki dla rodziny.

DANE RODZINY:
- Liczba osób: ${state.data.users.length}
- Budżet dzienny: ${state.data.household.dailyBudget} zł
- Preferencje: ${state.data.household.preferences || 'polska kuchnia, proste dania'}
- Alergie: ${state.data.household.allergies || 'brak'}

Zaproponuj konkretny obiad na dzisiaj. Format:

NAZWA DANIA:
[nazwa]

PRZEPIS:
[krótki przepis]

LISTA ZAKUPÓW:
- [produkt] - [ilość] - [cena zł]

KOSZT: [suma] zł`;

            try {
                const response = await fetch('https://api.anthropic.com/v1/messages', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        model: 'claude-sonnet-4-20250514',
                        max_tokens: 1000,
                        messages: [{ role: 'user', content: prompt }]
                    })
                });

                const result = await response.json();
                
                if (result.content && result.content[0]) {
                    state.aiResponse = result.content[0].text;
                    state.data.mealPlans.unshift({
                        content: result.content[0].text,
                        date: new Date().toISOString().split('T')[0],
                        id: Date.now()
                    });
                    saveData();
                }
            } catch (error) {
                state.aiResponse = '❌ Nie udało się połączyć z AI. Sprawdź internet.';
            }
            
            state.isLoadingAI = false;
            render();
        }

        // Renderowanie
        function render() {
            const stats = calculateStats();
            const app = document.getElementById('app');
            
            let content = '';

            if (!state.currentUser) {
                content = `
                    <div class="min-h-screen flex items-center justify-center p-4">
                        <div class="bg-white rounded-xl p-6 shadow-lg max-w-md w-full">
                            <h2 class="text-xl font-bold mb-4 flex items-center gap-2">
                                ${icons.users}
                                Wybierz użytkownika
                            </h2>
                            ${state.data.users.map(user => `
                                <button onclick="selectUser('${user}')" class="w-full p-3 mb-2 bg-indigo-50 rounded-lg hover:bg-indigo-100 transition text-left font-semibold">
                                    ${user}
                                </button>
                            `).join('')}
                            <div class="border-t pt-4 mt-4">
                                <input id="newUserInput" type="text" placeholder="Wpisz imię..." class="w-full p-3 border rounded-lg mb-2">
                                <p class="text-sm text-gray-500">Naciśnij Enter aby dodać</p>
                            </div>
                        </div>
                    </div>
                `;
            } else {
                content = `
                    <div class="min-h-screen pb-20">
                        <div class="bg-gradient-to-r from-indigo-600 to-blue-600 text-white p-6 shadow-lg">
                            <h1 class="text-2xl font-bold flex items-center gap-2">
                                ${icons.dollar}
                                Asystent Budżetowy Pro
                            </h1>
                            <div class="mt-2 flex items-center gap-2 text-sm">
                                ${icons.users}
                                <span>Zalogowany: <strong>${state.currentUser}</strong></span>
                            </div>
                        </div>

                        <div class="p-4">
                            ${state.activeTab === 'home' ? renderHome(stats) : ''}
                            ${state.activeTab === 'add' ? renderAdd() : ''}
                            ${state.activeTab === 'goals' ? renderGoals() : ''}
                            ${state.activeTab === 'meals' ? renderMeals() : ''}
                            ${state.activeTab === 'settings' ? renderSettings() : ''}
                        </div>

                        <div class="fixed bottom-0 left-0 right-0 bg-white border-t shadow-lg">
                            <div class="grid grid-cols-5 gap-1">
                                ${renderNavButton('home', icons.home, 'Start')}
                                ${renderNavButton('add', icons.plus, 'Dodaj')}
                                ${renderNavButton('goals', icons.target, 'Cele')}
                                ${renderNavButton('meals', icons.utensils, 'Posiłki')}
                                ${renderNavButton('settings', icons.settings, 'Ustawienia')}
                            </div>
                        </div>
                    </div>
                `;
            }

            app.innerHTML = content;

            // Event listeners dla nowego użytkownika
            const newUserInput = document.getElementById('newUserInput');
            if (newUserInput) {
                newUserInput.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter' && e.target.value) {
                        addUser(e.target.value);
                    }
                });
            }
        }

        function renderHome(stats) {
            return `
                <div class="space-y-4">
                    <div class="grid grid-cols-3 gap-3">
                        <div class="bg-white rounded-xl p-4 shadow-md">
                            <div class="text-xs text-gray-500 mb-1">Dziś</div>
                            <div class="text-xl font-bold text-indigo-600">${stats.todayTotal.toFixed(2)} zł</div>
                        </div>
                        <div class="bg-white rounded-xl p-4 shadow-md">
                            <div class="text-xs text-gray-500 mb-1">Tydzień</div>
                            <div class="text-xl font-bold text-blue-600">${stats.weekTotal.toFixed(2)} zł</div>
                        </div>
                        <div class="bg-white rounded-xl p-4 shadow-md">
                            <div class="text-xs text-gray-500 mb-1">Miesiąc</div>
                            <div class="text-xl font-bold text-purple-600">${stats.monthTotal.toFixed(2)} zł</div>
                        </div>
                    </div>

                    <div class="bg-white rounded-xl p-5 shadow-md">
                        <h3 class="font-bold text-lg mb-3 flex items-center gap-2">
                            ${icons.chart}
                            Wydatki według kategorii
                        </h3>
                        <div class="space-y-2">
                            ${categories.map(cat => {
                                const amount = stats.byCategory[cat] || 0;
                                const percent = stats.monthTotal > 0 ? (amount / stats.monthTotal * 100) : 0;
                                return `
                                    <div>
                                        <div class="flex justify-between text-sm mb-1">
                                            <span>${cat}</span>
                                            <span class="font-semibold">${amount.toFixed(2)} zł</span>
                                        </div>
                                        <div class="bg-gray-200 rounded-full h-2">
                                            <div class="bg-indigo-600 h-2 rounded-full transition-all" style="width: ${Math.min(percent, 100)}%"></div>
                                        </div>
                                    </div>
                                `;
                            }).join('')}
                        </div>
                    </div>

                    ${state.data.goals.length > 0 ? `
                        <div class="bg-white rounded-xl p-5 shadow-md">
                            <h3 class="font-bold text-lg mb-3 flex items-center gap-2">
                                ${icons.target}
                                Cele oszczędnościowe
                            </h3>
                            <div class="space-y-3">
                                ${state.data.goals.map(goal => {
                                    const progress = (goal.saved / goal.target * 100).toFixed(0);
                                    return `
                                        <div class="border-b pb-3 last:border-0">
                                            <div class="flex justify-between mb-2">
                                                <span class="font-semibold">${goal.name}</span>
                                                <span class="text-sm text-gray-600">${goal.saved} / ${goal.target} zł</span>
                                            </div>
                                            <div class="bg-gray-200 rounded-full h-3 mb-1">
                                                <div class="bg-green-600 h-3 rounded-full" style="width: ${Math.min(progress, 100)}%"></div>
                                            </div>
                                            <div class="flex justify-between text-xs text-gray-500">
                                                <span>${progress}% ukończone</span>
                                                ${goal.deadline ? `<span>Do: ${goal.deadline}</span>` : ''}
                                            </div>
                                        </div>
                                    `;
                                }).join('')}
                            </div>
                        </div>
                    ` : ''}
                </div>
            `;
        }

        function renderAdd() {
            return `
                <div class="bg-white rounded-xl p-5 shadow-md">
                    <h2 class="text-xl font-bold mb-4 flex items-center gap-2">
                        ${icons.plus}
                        Dodaj wydatek
                    </h2>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Data</label>
                            <input type="date" id="expenseDate" value="${state.newExpense.date}" class="w-full p-3 border rounded-lg">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Kategoria</label>
                            <select id="expenseCategory" class="w-full p-3 border rounded-lg">
                                ${categories.map(cat => `<option value="${cat}" ${state.newExpense.category === cat ? 'selected' : ''}>${cat}</option>`).join('')}
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Nazwa wydatku</label>
                            <input type="text" id="expenseName" value="${state.newExpense.name}" placeholder="np. Zakupy spożywcze" class="w-full p-3 border rounded-lg">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Kwota (zł)</label>
                            <input type="number" step="0.01" id="expenseAmount" value="${state.newExpense.amount}" placeholder="0.00" class="w-full p-3 border rounded-lg">
                        </div>
                        <button onclick="submitExpense()" class="w-full bg-indigo-600 text-white py-3 rounded-lg font-semibold hover:bg-indigo-700">
                            Dodaj wydatek
                        </button>
                    </div>
                </div>
            `;
        }

        function renderGoals() {
            return `
                <div class="space-y-4">
                    <div class="bg-white rounded-xl p-5 shadow-md">
                        <h2 class="text-xl font-bold mb-4 flex items-center gap-2">
                            ${icons.target}
                            Dodaj cel oszczędnościowy
                        </h2>
                        <div class="space-y-4">
                            <input type="text" id="goalName" value="${state.newGoal.name}" placeholder="Nazwa celu (np. Wakacje)" class="w-full p-3 border rounded-lg">
                            <input type="number" id="goalTarget" value="${state.newGoal.target}" placeholder="Kwota do zaoszczędzenia (zł)" class="w-full p-3 border rounded-lg">
                            <input type="date" id="goalDeadline" value="${state.newGoal.deadline}" class="w-full p-3 border rounded-lg">
                            <button onclick="submitGoal()" class="w-full bg-green-600 text-white py-3 rounded-lg font-semibold">Dodaj cel</button>
                        </div>
                    </div>

                    ${state.data.goals.map(goal => `
                        <div class="bg-white rounded-xl p-5 shadow-md">
                            <h3 class="font-bold text-lg mb-3">${goal.name}</h3>
                            <div class="mb-4">
                                <div class="flex justify-between mb-2">
                                    <span>Postęp</span>
                                    <span class="font-bold">${goal.saved} / ${goal.target} zł</span>
                                </div>
                                <div class="bg-gray-200 rounded-full h-4">
                                    <div class="bg-green-600 h-4 rounded-full" style="width: ${Math.min((goal.saved / goal.target * 100), 100)}%"></div>
                                </div>
                            </div>
                            <div class="flex gap-2">
                                <input type="number" id="goalInput${goal.id}" placeholder="Kwota" class="flex-1 p-2 border rounded-lg text-sm">
                                <button onclick="addToGoal(${goal.id})" class="px-4 py-2 bg-green-600 text-white rounded-lg text-sm font-semibold">Dodaj</button>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
        }

        function renderMeals() {
            return `
                <div class="space-y-4">
                    <div class="bg-white rounded-xl p-5 shadow-md">
                        <h2 class="text-xl font-bold mb-3 flex items-center gap-2">
                            ${icons.sparkles}
                            Generator posiłków AI
                        </h2>
                        <div class="mb-4 p-3 bg-blue-50 rounded-lg text-sm">
                            <div class="font-semibold mb-1">Ustawienia rodziny:</div>
                            <div>Osób: ${state.data.users.length}</div>
                            <div>Budżet: ${state.data.household.dailyBudget} zł</div>
                        </div>
                        <button onclick="generateAIMealPlan()" ${state.isLoadingAI ? 'disabled' : ''} class="w-full bg-purple-600 text-white py-3 rounded-lg font-semibold hover:bg-purple-700 disabled:bg-gray-400 flex items-center justify-center gap-2">
                            ${icons.sparkles}
                            <span>${state.isLoadingAI ? 'Generuję z AI...' : 'Wygeneruj plan AI'}</span>
                        </button>
                        
                        ${state.aiResponse ? `
                            <div class="mt-4 p-4 bg-green-50 rounded-lg border border-green-200">
                                <pre class="whitespace-pre-wrap text-sm">${state.aiResponse}</pre>
                            </div>
                        ` : ''}
                    </div>

                    <div class="bg-yellow-50 border border-yellow-200 rounded-xl p-4 text-sm">
                        <p class="font-semibold mb-2">💡 Jak działa darmowe AI:</p>
                        <p>Ta aplikacja używa API Claude (Anthropic) do generowania posiłków!</p>
                    </div>

                    ${state.data.mealPlans.length > 0 ? `
                        <div class="space-y-3">
                            <h3 class="font-bold">Historia planów:</h3>
                            ${state.data.mealPlans.map(plan => `
                                <div class="bg-white rounded-xl p-4 shadow-md">
                                    <div class="text-xs text-gray-500 mb-2">${plan.date}</div>
                                    <pre class="whitespace-pre-wrap text-sm">${plan.content}</pre>
                                </div>
                            `).join('')}
                        </div>
                    ` : ''}
                </div>
            `;
        }

        function renderSettings() {
            return `
                <div class="space-y-4">
                    <div class="bg-white rounded-xl p-5 shadow-md">
                        <h2 class="text-xl font-bold mb-4 flex items-center gap-2">
                            ${icons.settings}
                            Ustawienia
                        </h2>
                        
                        <div class="space-y-4">
                            <div>
                                <label class="block text-sm font-medium mb-2">Budżet dzienny (zł)</label>
                                <input type="number" id="dailyBudget" value="${state.data.household.dailyBudget}" class="w-full p-3 border rounded-lg">
                            </div>

                            <div>
                                <label class="block text-sm font-medium mb-2">Preferencje żywieniowe</label>
                                <input type="text" id="preferences" value="${state.data.household.preferences}" placeholder="np. polska kuchnia" class="w-full p-3 border rounded-lg">
                            </div>

                            <div>
                                <label class="block text-sm font-medium mb-2">Alergie/ograniczenia</label>
                                <input type="text" id="allergies" value="${state.data.household.allergies}" placeholder="np. gluten, orzechy" class="w-full p-3 border rounded-lg">
                            </div>

                            <button onclick="saveSettings()" class="w-full bg-blue-600 text-white py-3 rounded-lg font-semibold">Zapisz ustawienia</button>

                            <div class="border-t pt-4">
                                <h3 class="font-semibold mb-3 flex items-center gap-2">
                                    ${icons.bell}
                                    Powiadomienia
                                </h3>
                                <div class="space-y-2">
                                    <label class="flex items-center gap-2">
                                        <input type="checkbox" id="notifDaily" ${state.data.notifications.dailyReminder ? 'checked' : ''} class="w-5 h-5">
                                        <span>Codzienne przypomnienia</span>
                                    </label>
                                    <label class="flex items-center gap-2">
                                        <input type="checkbox" id="notifBudget" ${state.data.notifications.budgetAlert ? 'checked' : ''} class="w-5 h-5">
                                        <span>Alerty budżetowe</span>
                                    </label>
                                    <label class="flex items-center gap-2">
                                        <input type="checkbox" id="notifGoals" ${state.data.notifications.goalProgress ? 'checked' : ''} class="w-5 h-5">
                                        <span>Postęp celów</span>
                                    </label>
                                </div>
                            </div>

                            <div class="border-t pt-4">
                                <h3 class="font-semibold mb-2">Użytkownicy:</h3>
                                <div class="space-y-2 mb-3">
                                    ${state.data.users.map(user => `
                                        <div class="flex justify-between items-center p-2 bg-gray-50 rounded">
                                            <span>${user}</span>
                                            <button onclick="removeUser('${user}')" class="text-red-600 text-sm">
                                                ${icons.trash}
                                            </button>
                                        </div>
                                    `).join('')}
                                </div>
                                <button onclick="logout()" class="w-full bg-blue-600 text-white py-2 rounded-lg text-sm">Dodaj użytkownika</button>
                            </div>

                            <div class="border-t pt-4">
                                <button onclick="clearAllData()" class="w-full bg-red-500 text-white py-3 rounded-lg font-semibold">Wyczyść wszystkie dane</button>
                            </div>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderNavButton(tab, icon, label) {
            const active = state.activeTab === tab;
            return `
                <button onclick="changeTab('${tab}')" class="p-3 flex flex-col items-center ${active ? 'text-indigo-600' : 'text-gray-400'}">
                    ${icon}
                    <span class="text-xs mt-1">${label}</span>
                </button>
            `;
        }

        // Funkcje globalne
        window.selectUser = (name) => {
            state.currentUser = name;
            render();
        };

        window.logout = () => {
            state.currentUser = '';
            render();
        };

        window.changeTab = (tab) => {
            state.activeTab = tab;
            render();
        };

        window.submitExpense = () => {
            state.newExpense.date = document.getElementById('expenseDate').value;
            state.newExpense.category = document.getElementById('expenseCategory').value;
            state.newExpense.name = document.getElementById('expenseName').value;
            state.newExpense.amount = document.getElementById('expenseAmount').value;
            addExpense();
        };

        window.submitGoal = () => {
            state.newGoal.name = document.getElementById('goalName').value;
            state.newGoal.target = document.getElementById('goalTarget').value;
            state.newGoal.deadline = document.getElementById('goalDeadline').value;
            addGoal();
        };

        window.addToGoal = (goalId) => {
            const input = document.getElementById(`goalInput${goalId}`);
            if (input && input.value) {
                updateGoalProgress(goalId, input.value);
            }
        };

        window.saveSettings = () => {
            state.data.household.dailyBudget = parseFloat(document.getElementById('dailyBudget').value);
            state.data.household.preferences = document.getElementById('preferences').value;
            state.data.household.allergies = document.getElementById('allergies').value;
            state.data.notifications.dailyReminder = document.getElementById('notifDaily').checked;
            state.data.notifications.budgetAlert = document.getElementById('notifBudget').checked;
            state.data.notifications.goalProgress = document.getElementById('notifGoals').checked;
            saveData();
            alert('✅ Ustawienia zapisane!');
            render();
        };

        window.removeUser = (name) => {
            if (confirm(`Usunąć użytkownika ${name}?`)) {
                state.data.users = state.data.users.filter(u => u !== name);
                if (state.currentUser === name) state.currentUser = '';
                saveData();
                render();
            }
        };

        window.clearAllData = () => {
            if (confirm('Wyczyścić WSZYSTKIE dane? To nie może być cofnięte!')) {
                localStorage.clear();
                location.reload();
            }
        };

        // Start aplikacji
        loadData();
        render();
    </script>
</body>
</html>
