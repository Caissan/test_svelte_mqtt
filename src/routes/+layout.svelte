<script>
	import './styles.css';
</script>

<div class="flex flex-col h-full justify-between items-center py-3">
	<header class="text-2xl">
		Тестовое задание
	</header>

	<main class="">
		<slot />
	</main>

	<footer>
		<p>Игнатьев Тимур
			| <a class="text-blue-600" target="_blank" href="https://spb.hh.ru/resume/98eeebd1ff0bddd94b0039ed1f313264454278">резюме</a>
			| <a class="text-blue-600" target="_blank" href="https://github.com/Caissan">github</a>
			| <a class="text-blue-600" target="_blank" href="https://t.me/Caissann">телеграм</a></p>
	</footer>
</div>
