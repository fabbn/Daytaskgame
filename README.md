import { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Progress } from '@/components/ui/progress';

const dailyQuests = [
  { id: 'admin', label: 'Administratie bijgewerkt?' },
  { id: 'clean', label: 'Opruimen na werk gedaan?' },
];

const weeklyQuest = {
  id: 'brainstorm',
  label: '1 uur brainstormen voor De Wormshoef',
};

const rewards = [
  { xp: 50, label: '1 uur gamen' },
  { xp: 100, label: 'Snack of traktatie' },
  { xp: 200, label: 'Mini-uitje of me-time' },
];

export default function DayTaskGame() {
  const [xp, setXp] = useState(0);
  const [daily, setDaily] = useState({ admin: false, clean: false });
  const [weekly, setWeekly] = useState(false);

  const handleDailyToggle = (id) => {
    const updated = { ...daily, [id]: !daily[id] };
    setDaily(updated);
    if (!daily[id]) setXp((prev) => prev + 10);
  };

  const handleWeeklyToggle = () => {
    if (!weekly) setXp((prev) => prev + 25);
    setWeekly(!weekly);
  };

  const level = Math.floor(xp / 100);
  const nextRewardXp = rewards.find((r) => r.xp > xp)?.xp || 200;

  return (
    <div className="p-4 space-y-4 max-w-md mx-auto">
      <h1 className="text-2xl font-bold text-center">DayTaskGame</h1>

      <Card>
        <CardContent className="p-4 space-y-2">
          <h2 className="text-xl font-semibold">XP: {xp} (Level {level})</h2>
          <Progress value={(xp % 100)} max={100} />
        </CardContent>
      </Card>

      <Card>
        <CardContent className="p-4 space-y-2">
          <h2 className="font-semibold">Daily Quests</h2>
          {dailyQuests.map((quest) => (
            <Button
              key={quest.id}
              onClick={() => handleDailyToggle(quest.id)}
              variant={daily[quest.id] ? 'default' : 'outline'}
              className="w-full"
            >
              {quest.label}
            </Button>
          ))}
        </CardContent>
      </Card>

      <Card>
        <CardContent className="p-4 space-y-2">
          <h2 className="font-semibold">Weekly Boss</h2>
          <Button
            onClick={handleWeeklyToggle}
            variant={weekly ? 'default' : 'outline'}
            className="w-full"
          >
            {weeklyQuest.label}
          </Button>
        </CardContent>
      </Card>

      <Card>
        <CardContent className="p-4 space-y-2">
          <h2 className="font-semibold">Beloningen</h2>
          <ul className="list-disc list-inside">
            {rewards.map((r) => (
              <li key={r.xp}>
                {r.label} ({r.xp} XP)
              </li>
            ))}
          </ul>
          <p className="text-sm text-muted-foreground">
            Nog {nextRewardXp - xp} XP tot volgende beloning.
          </p>
        </CardContent>
      </Card>
    </div>
  );
}
