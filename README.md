# Medico 
# A new aspect towards medicine inventory management system

   

<div> <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Admin%20Login.png" width="400" height="200" />
<img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Pharmacist.png" width="400" height="200" />
</div>
<div> <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Doctor%20Login.png" width="400" height="200" />
<img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Receptionist%20Login.png" width="400" height="200" />
    <img src="https://github.com/tech-jamara/PharmacyManagementSystem/blob/main/screenshot/Patient%20login.png" width="400" height="200" />
</div>

--------------------------------------------------------------------------------------
# Create a new medicine
```@app.route(/medicine, methods=[POST])
def add_medicine():
name = request.json[name]
manufacturer = request.json[manufacturer]
quantity = request.json[quantity]

medicine = {
name: name,
manufacturer: manufacturer,
quantity: quantity
}

mongo.db.medicines.insert_one(medicine)

return jsonify({message: Medicine added successfully}), 201
```

# Get all medicines
```@app.route(/medicines, methods=[GET])
def get_medicines():
medicines = mongo.db.medicines.find()
result = []
for medicine in medicines:
result.append({
id: str(medicine[_id]),
name: medicine[name],
manufacturer: medicine[manufacturer],
quantity: medicine[quantity]
})
return jsonify(result)
```

# Get a single medicine by ID
```@app.route(/medicine/&ltid&gt, methods=[GET])
def get_medicine(id):
medicine = mongo.db.medicines.find_one({_id: ObjectId(id)})
if medicine:
return jsonify({
id: str(medicine[_id]),
name: medicine[name],
manufacturer: medicine[manufacturer],
quantity: medicine[quantity]
})
return jsonify({message: Medicine not found}), 404
```

# Update a medicine by ID
```@app.route(/medicine/&ltid&gt, methods=[PUT])
def update_medicine(id):
name = request.json[name]
manufacturer = request.json[manufacturer]
quantity = request.json[quantity]

mongo.db.medicines.update_one({_id: ObjectId(id)}, {$set: {
name: name,
manufacturer: manufacturer,
quantity: quantity
}})

return jsonify({message: Medicine updated successfully})
```

# Delete a medicine by ID
```
@app.route(/medicine/&ltid&gt, methods=[DELETE])
def delete_medicine(id):
mongo.db.medicines.delete_one({_id: ObjectId(id)})
return jsonify({message: Medicine deleted successfully})
```

# Search Medicines by Name:
```
@app.route(/medicines/search/&ltkeyword&gt, methods=[GET])
def search_medicines(keyword):
medicines = mongo.db.medicines.find({&quotname&quot: {&quot$regex&quot: keyword, &quot$options&quot: &quoti&quot}})
result = []
for medicine in medicines:
result.append({
id: str(medicine[_id]),
name: medicine[name],
manufacturer: medicine[manufacturer],
quantity: medicine[quantity]
})
return jsonify(result)
```

# Sort Medicines by Quantity:
```
@app.route(/medicines/sort/quantity, methods=[GET])
def sort_medicines_by_quantity():
medicines = mongo.db.medicines.find().sort(quantity, 1) # 1 for ascending, -1 for
descending
result = []
for medicine in medicines:
result.append({
id: str(medicine[_id]),
name: medicine[name],
manufacturer: medicine[manufacturer],
quantity: medicine[quantity]
})
return jsonify(result)
```

# Get Medicine Statistics (Total Quantity):
```
@app.route(/medicine/statistics, methods=[GET])
def get_medicine_statistics():
total_quantity = mongo.db.medicines.aggregate([
{&quot$group&quot: {&quot_id&quot: None, &quottotal_quantity&quot: {&quot$sum&quot: &quot$quantity&quot}}}
])
total_quantity = list(total_quantity)[0][total_quantity] if total_quantity else 0
return jsonify({total_quantity: total_quantity})
```










