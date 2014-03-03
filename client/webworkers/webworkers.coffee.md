		
	fiboWorkers = new Array()

	stopWorkers = ->
		console.log "stop workers"
		worker.terminate() for worker in fiboWorkers
		fiboWorkers = new Array()


	initWorkers = ->
		console.log "init workers"
		fiboWorkers = 
			for i in [1..8]
				do (i)->
					worker = new FiboWorker()
					worker.onmessage = (e) -> console.log "Worker #{i}:", e.data
					worker

	runWorkers = ->
		initWorkers() if _(fiboWorkers).size() == 0 
		console.log "run workers"
		for i in [1..60]
			worker = fiboWorkers[i%fiboWorkers.length]
			worker.postMessage i
	
	
	Router.map -> @route 'webworkers', load: initWorkers, unload: stopWorkers, after: runWorkers
		
	