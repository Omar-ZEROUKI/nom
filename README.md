let debounceTimeout;

document.getElementById('location').addEventListener('input', function () {
    const query = this.value;

    clearTimeout(debounceTimeout);

    if (query.length < 2) return;

    debounceTimeout = setTimeout(async () => {
        try {
            const response = await fetch(`/autocomplete?query=${encodeURIComponent(query)}`);
            const suggestions = await response.json();

            const suggestionList = document.getElementById('suggestions');
            suggestionList.innerHTML = '';

            suggestions.forEach(item => {
                const li = document.createElement('li');
                li.textContent = item;
                li.onclick = () => {
                    document.getElementById('location').value = item;
                    suggestionList.innerHTML = '';
                };
                suggestionList.appendChild(li);
            });
        } catch (error) {
            console.error('Erreur lors de la récupération des suggestions :', error);
        }
    }, 3000); // 3 secondes
});
