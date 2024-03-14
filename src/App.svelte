<script>
	import {
		Column,
		ColumnType,
		Index,
		IndexedColumn,
		Schema,
		Table,
		WASQLitePowerSyncDatabaseOpenFactory,
	} from '@journeyapps/powersync-sdk-web';

	export const AppSchema = new Schema([
		new Table({
			name: 'todos',
			columns: [
				new Column({ name: 'list_id', type: ColumnType.TEXT }),
				new Column({ name: 'created_at', type: ColumnType.TEXT }),
				new Column({ name: 'completed_at', type: ColumnType.TEXT }),
				new Column({ name: 'description', type: ColumnType.TEXT }),
				new Column({ name: 'completed', type: ColumnType.INTEGER }),
				new Column({ name: 'created_by', type: ColumnType.TEXT }),
				new Column({ name: 'completed_by', type: ColumnType.TEXT })
			],
			indexes: [new Index({ name: 'list', columns: [new IndexedColumn({ name: 'list_id' })] })]
		}),
		new Table({
			name: 'lists',
			columns: [
				new Column({ name: 'id', type: ColumnType.TEXT }),
				new Column({ name: 'name', type: ColumnType.TEXT }),
			]
		})
	]);

	const factory = new WASQLitePowerSyncDatabaseOpenFactory({
		schema: AppSchema, // Which was created earlier
		dbFilename: 'test.sqlite'
	});

	let PowerSync = factory.getInstance();

	async function setupPowerSync() {
		let result

		console.log('before powersync init')

		await PowerSync.init();

		console.log('after powersync init')

		result = await PowerSync.execute(`insert into lists (id, name) values (uuid(), 'list0')`)
		console.log('insert result: ', result)

		result = await PowerSync.execute('select * from lists')
		console.log('select result: ', result)
	}

	setupPowerSync()
</script>

<main class="container">
	Demo of tauri + svelte + powersync
</main>
