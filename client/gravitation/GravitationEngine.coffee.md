	
gravitation constant

	G = 1

	step = (objects, dt) ->

because we do an equals in getAcceleration we can compare the object to itself
otherwise, we should leave out the object itself in the array passed to calculateNewPosition

		calculateNewPosition obj, objects, dt for obj in objects


##Position
	
	calculateNewPosition = (obj, otherObjects, dt) ->

acceleration of an object is given by newtons gravitation law

		acc = getAcceleration obj, otherObjects

velocity is acceleration * dt
we can overwrite acc, so we use the underscore method
		
		obj.v._add acc._multi dt

and the position is veolicty * dt
we cant change obj.v here, so we have to return a new vector, so no underscore

		obj.x._add obj.v.multi dt

##Acceleration
here we ignore to objects that are considered equal in its position x. it would create floating point issues or divisions by zero

	getAcceleration = (obj, otherObjects) ->
		a_total = new Vector obj.x.data.length
		for otherObj in otherObjects
			unless obj.x.equals otherObj.x
				acc = obj.x.sub otherObj.x
				acc._multi -G * otherObj.m*(1/acc.length() ** 3)
				a_total._add acc
		return a_total

exports
	
	@GravitationEngine = 
		step: step
