import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { motion } from "framer-motion";

export default function HoursCalculator() { const [entries, setEntries] = useState([{ start: "", end: "" }]); const [totalHours, setTotalHours] = useState(0);

const handleChange = (index, field, value) => { const newEntries = [...entries]; newEntries[index][field] = value; setEntries(newEntries); };

const addEntry = () => { setEntries([...entries, { start: "", end: "" }]); };

const calculateTotalHours = () => { let total = 0; entries.forEach(({ start, end }) => { if (start && end) { const startDate = new Date(1970-01-01T${start}:00); const endDate = new Date(1970-01-01T${end}:00); const diff = (endDate - startDate) / (1000 * 60 * 60); total += diff > 0 ? diff : 24 + diff; // Handles overnight shifts } }); setTotalHours(total); };

return ( <div className="p-6 max-w-3xl mx-auto space-y-6"> <motion.h1 className="text-4xl font-bold text-center" initial={{ opacity: 0, y: -20 }} animate={{ opacity: 1, y: 0 }} > Hours Calculator </motion.h1>

{entries.map((entry, index) => (
    <Card key={index} className="flex gap-4 p-4 items-center">
      <CardContent className="flex w-full gap-4">
        <Input
          type="time"
          value={entry.start}
          onChange={(e) => handleChange(index, "start", e.target.value)}
        />
        <Input
          type="time"
          value={entry.end}
          onChange={(e) => handleChange(index, "end", e.target.value)}
        />
      </CardContent>
    </Card>
  ))}

  <div className="flex justify-center gap-4">
    <Button onClick={addEntry}>Add Entry</Button>
    <Button onClick={calculateTotalHours}>Calculate Total</Button>
  </div>

  <motion.div
    className="text-2xl font-semibold text-center mt-4"
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
  >
    Total Hours: {totalHours.toFixed(2)}
  </motion.div>
</div>

); }

